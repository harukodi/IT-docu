
## Put the following content in your nextclouds config.php file 
```php
  'allow_user_to_change_display_name' => false,
  'lost_password_link' => 'disabled',
  'oidc_login_provider_url' => 'https://auth.domain.tld/application/o/<your-application-slug-goes-here>/',
  'oidc_login_client_id' => 'client-id-goes-here',
  'oidc_login_client_secret' => 'client-secret-goes-here',
  'oidc_login_auto_redirect' => false,
  'oidc_login_end_session_redirect' => false,
  'oidc_login_button_text' => 'Använd Authentik',
  'oidc_login_hide_password_form' => false,
  'oidc_login_use_id_token' => true,
  'oidc_login_attributes' => array (
      'id' => 'preferred_username',
      'name' => 'name',
      'mail' => 'email',
      'groups' => 'groups',
  ),
  'oidc_login_use_external_storage' => true,
  'oidc_login_scope' => 'openid profile email groups',
  'oidc_login_proxy_ldap' => false,
  'oidc_login_disable_registration' => false,
  'oidc_login_redir_fallback' => false,
  'oidc_login_tls_verify' => true,
  'oidc_create_groups' => true,
  'oidc_login_webdav_enabled' => false,
  'oidc_login_password_authentication' => false,
  'oidc_login_public_key_caching_time' => 86400,
  'oidc_login_min_time_between_jwks_requests' => 10,
  'oidc_login_well_known_caching_time' => 86400,
  'oidc_login_update_avatar' => false,
  'oidc_login_default_quota' => '1073741824',
```

## To change default quota you will have to change the following
```php
'oidc_login_default_quota' => 'your-custom-quota-goes-here',
```
> __NOTE__: The quota is specified using bytes. For example 1Gb = 1073741824 Bytes