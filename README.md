# Scrollable Date Picker for Svelte

A scrollable datepicker designed for touch screen interfaces with Svelte.

### Installation:

Add the package to your dependencies

NPM:

```bash
    npm install @10xminds/scrollable-date-picker

```

Yarn:

```bash
    yarn add @10xminds/scrollable-date-picker
```

```svelte
<script>
    import { ScrollableDatePicker } from "@10xminds/scrollable-date-picker";

    <ScrollableDatePicker
        selectedDate="2023-01-23"
        rowsShown={3}
        separatorWidth={4}
        rowBackgroundColor="#03FFFF"
    />
```

## TODO: Add more documentation with options

# Development Instructions

Instal dependencies with

```bash
npm install
```

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

Everything inside `src/lib` is part of your library, everything inside `src/routes` can be used as a showcase or preview app.

## Building

To build your library:

```bash
npm run package
```

To create a production version of your showcase app:

```bash
npm run build
```

## Publishing

To publish the library to [npm](https://www.npmjs.com):

```bash
npm publish
```
