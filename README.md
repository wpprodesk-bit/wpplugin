<?php
/**
 * Plugin Name: TeamVirtual Contract Logger
 * Description: Logs contract form submissions securely into a custom post type.
 * Version: 1.0
 * Author: Sakib Hossain
 * License: GPL2
 */

namespace TeamVirtual\ContractLogger;

defined('ABSPATH') || exit;

// Register Custom Post Type
add_action('init', function () {
    register_post_type('contract_log', [
        'label' => 'Contract Logs',
        'public' => false,
        'show_ui' => true,
        'supports' => ['title', 'custom-fields'],
    ]);
});

// Enqueue nonce field in form
add_action('wp_footer', function () {
    if (is_page('contract')) {
        wp_nonce_field('tv_contract_nonce_action', 'tv_contract_nonce_field');
    }
});

// Handle form submission
add_action('init', function () {
    if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['tv_contract_nonce_field'])) {
        if (!wp_verify_nonce($_POST['tv_contract_nonce_field'], 'tv_contract_nonce_action')) {
            wp_die('Security check failed');
        }

        $client_name = sanitize_text_field($_POST['client_name'] ?? '');
        $contract_type = sanitize_text_field($_POST['contract_type'] ?? '');
        $details = sanitize_textarea_field($_POST['details'] ?? '');

        $post_id = wp_insert_post([
            'post_type' => 'contract_log',
            'post_title' => $client_name . ' - ' . current_time('mysql'),
            'post_status' => 'publish',
        ]);

        if ($post_id && function_exists('update_field')) {
            update_field('field_client_name', $client_name, $post_id);
            update_field('field_contract_type', $contract_type, $post_id);
            update_field('field_details', $details, $post_id);
        }
    }
});
