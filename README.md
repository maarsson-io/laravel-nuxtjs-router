> [!WARNING]
> **This package is deprecated and no longer maintained.**
>
> Its functionality has been replaced by Nginx routing rules.
> The configuration below is preserved for future reference.

# NuxtJS router for Laravel

This package adds a fallback routing to your Laravel-NuxtJS project.

## Alternative: Nginx configuration

Instead of handling Laravel/Nuxt routing within the application,
Nginx can route requests directly to the appropriate destination.

The following configuration was used with Laravel 13, Nuxt 2
(static SPA), and Laravel Forge.
```nginx
# Laravel API
location ^~ /api/ {
    try_files $uri $uri/ /index.php?$query_string;
}

# Laravel Sanctum
location = /sanctum/csrf-cookie {
    try_files $uri /index.php?$query_string;
}

# Nuxt assets
location ^~ /_nuxt/ {
    alias /path/to/public/_nuxt/_nuxt/;
}

# Nuxt SPA fallback
location / {
    try_files $uri @nuxt;
}

location @nuxt {
    root /path/to/public/_nuxt;
    try_files $uri $uri/ /index.html;
}
```

Please note:
- **Nuxt 2:** Designed for a statically generated SPA, not server-side rendering (SSR).
- **Directory structure:** Adjust the `alias` and `root` paths to match your deployment structure.
- **Laravel routes:** Only `/api/*` and `/sanctum/csrf-cookie` are explicitly routed to Laravel. Other endpoints may require additional rules.
- **Nginx configuration:** This is an excerpt from a working Laravel Forge configuration, not a complete standalone configuration. The SPA fallback behavior depends on the surrounding Nginx configuration.

## Legacy Usage

1. Add package to your Laravel project: `composer require maarsson/laravel-nuxtjs-router`
2. Set up frontend generate folder path in your NuxtJS config in order to build the generated files under Laravels `public` folder. In your `nuxt.config.js`:
    ```
    generate: {
        dir: '../public/_nuxt'
    },
    ```
    You may change the relative path in the example above, according to your NuxtJS placement in your project.
3. Generate NuxtJs frontend.
4. Open your project in the browser.


## License

This package is open-sourced software licensed under the [MIT license](LICENSE.md).
