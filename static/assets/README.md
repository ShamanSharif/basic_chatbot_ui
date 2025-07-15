# Assets Directory

This directory contains static assets for the Chatbot application.

## Icon Files

- `app-icon.svg` - Main application icon (64x64px)
- Replace this with your actual application icon

## Usage

In your Svelte components, you can reference these assets using:

```svelte
<img src="/assets/app-icon.svg" alt="App Icon" />
```

## Recommended Icon Sizes

For a complete icon set, consider creating multiple sizes:
- `app-icon-16.svg` - 16x16px (favicon)
- `app-icon-32.svg` - 32x32px (small icon)
- `app-icon-64.svg` - 64x64px (standard app icon)
- `app-icon-128.svg` - 128x128px (large icon)
- `app-icon-512.svg` - 512x512px (PWA icon)

## File Formats

- **SVG** - Recommended for icons (scalable, small file size)
- **PNG** - For complex graphics or when SVG isn't supported
- **ICO** - For favicon files

## Updating the Navigation Component

To use your new icon in the Navigation component, update the logo section in `src/lib/components/Navigation.svelte`:

```svelte
<div class="flex items-center space-x-4">
  <img src="/assets/app-icon.svg" alt="Chatbot" class="h-8 w-8" />
  <div class="text-xl font-bold">Chatbot</div>
</div>
``` 