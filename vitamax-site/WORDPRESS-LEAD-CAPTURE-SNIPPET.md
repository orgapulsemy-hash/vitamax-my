# Lead Capture — WordPress Snippet (5 minutes)

The landing page now offers **The Royal Honey Handbook** in exchange for an email address
(inline band above the FAQ, plus an exit-intent popup). The form posts to WordPress.

**Until you add this snippet, the form still works for the visitor** — they always get the
Handbook — but the email is not stored anywhere. Add this and every address is saved and
emailed to you.

## Install

WordPress admin → **Snippets** → **Add New** → paste the code below → set to
**"Run everywhere"** → **Save and Activate**.

```php
<?php
/**
 * VitaMAX — capture Handbook leads from the landing page.
 * Stores each email in the wp_options table and notifies the shop owner.
 */
add_action( 'wp_ajax_vitamax_lead',        'vitamax_capture_lead' );
add_action( 'wp_ajax_nopriv_vitamax_lead', 'vitamax_capture_lead' );

function vitamax_capture_lead() {

    $email  = isset( $_POST['email'] )  ? sanitize_email( wp_unslash( $_POST['email'] ) )        : '';
    $source = isset( $_POST['source'] ) ? sanitize_text_field( wp_unslash( $_POST['source'] ) )  : 'unknown';

    if ( ! is_email( $email ) ) {
        wp_send_json_error( array( 'message' => 'Invalid email' ), 400 );
    }

    // Basic flood protection: max 5 submissions per IP per hour.
    $ip  = isset( $_SERVER['REMOTE_ADDR'] ) ? sanitize_text_field( wp_unslash( $_SERVER['REMOTE_ADDR'] ) ) : '';
    $key = 'vmx_lead_rate_' . md5( $ip );
    $hits = (int) get_transient( $key );
    if ( $hits >= 5 ) {
        wp_send_json_error( array( 'message' => 'Too many requests' ), 429 );
    }
    set_transient( $key, $hits + 1, HOUR_IN_SECONDS );

    $leads = get_option( 'vitamax_leads', array() );
    if ( ! is_array( $leads ) ) {
        $leads = array();
    }

    // Skip duplicates, but still return success so the visitor gets the guide.
    foreach ( $leads as $existing ) {
        if ( isset( $existing['email'] ) && strtolower( $existing['email'] ) === strtolower( $email ) ) {
            wp_send_json_success( array( 'message' => 'Already subscribed' ) );
        }
    }

    $leads[] = array(
        'email'  => $email,
        'source' => $source,
        'date'   => current_time( 'mysql' ),
    );
    update_option( 'vitamax_leads', $leads, false );

    // Notify the shop owner.
    wp_mail(
        get_option( 'admin_email' ),
        'New VitaMAX lead: ' . $email,
        "Email: {$email}\nSource: {$source}\nDate: " . current_time( 'mysql' ) . "\nTotal leads: " . count( $leads )
    );

    wp_send_json_success( array( 'message' => 'Saved' ) );
}

/**
 * Tools -> VitaMAX Leads : view the list and download it as CSV.
 */
add_action( 'admin_menu', function () {
    add_management_page( 'VitaMAX Leads', 'VitaMAX Leads', 'manage_options', 'vitamax-leads', 'vitamax_leads_page' );
} );

function vitamax_leads_page() {
    if ( ! current_user_can( 'manage_options' ) ) {
        return;
    }
    $leads = get_option( 'vitamax_leads', array() );
    if ( ! is_array( $leads ) ) {
        $leads = array();
    }

    // CSV export
    if ( isset( $_GET['export'] ) && check_admin_referer( 'vmx_export' ) ) {
        header( 'Content-Type: text/csv' );
        header( 'Content-Disposition: attachment; filename=vitamax-leads.csv' );
        $out = fopen( 'php://output', 'w' );
        fputcsv( $out, array( 'Email', 'Source', 'Date' ) );
        foreach ( $leads as $l ) {
            fputcsv( $out, array( $l['email'], $l['source'], $l['date'] ) );
        }
        fclose( $out );
        exit;
    }

    $url = wp_nonce_url( admin_url( 'tools.php?page=vitamax-leads&export=1' ), 'vmx_export' );
    echo '<div class="wrap"><h1>VitaMAX Leads (' . count( $leads ) . ')</h1>';
    echo '<p><a class="button button-primary" href="' . esc_url( $url ) . '">Download CSV</a></p>';
    echo '<table class="widefat striped"><thead><tr><th>Email</th><th>Source</th><th>Date</th></tr></thead><tbody>';
    foreach ( array_reverse( $leads ) as $l ) {
        echo '<tr><td>' . esc_html( $l['email'] ) . '</td><td>' . esc_html( $l['source'] ) . '</td><td>' . esc_html( $l['date'] ) . '</td></tr>';
    }
    echo '</tbody></table></div>';
}
```

## Where the emails go

- **WordPress admin → Tools → VitaMAX Leads** — full list, with a **Download CSV** button
- You also get an email notification for each new lead

## When the list grows

Export the CSV and import it into a proper email tool (Mailchimp, Klaviyo, Brevo) so you can
send sequences rather than one-off blasts. At that point tell me and I'll point the form
straight at that service instead.

## Also worth doing

Send the Handbook link in your **WooCommerce order confirmation email** as well —
`https://vitamax.my/royal-honey-handbook.html`. It's promised in the offer stack on every
product page, and nothing sends it automatically yet.
