# Test reCAPTCHA Repository

This repository is intended for testing Google reCAPTCHA in a local environment.

## What this repository contains

- `test_v2.html` - Example HTML page for testing reCAPTCHA
- `test_v3.html` - Example HTML page for testing reCAPTCHA v3
- `prompt.txt` - Instruction text for generating this README

## What you need before testing

1. A Google reCAPTCHA site key
2. A Google reCAPTCHA secret key

Configure those keys in the HTML file(s) before testing if they are not already set.

## How the test works

When the HTML page loads, it should trigger reCAPTCHA and display a token in the browser console. That token is the reCAPTCHA response that can later be verified with Google using your secret key.

## Setup local alias for example.com

This example uses `example.com` mapped to `localhost` so the page can load as if it were hosted on a real domain.

### Windows

1. Open Notepad as Administrator.
2. Open the file:
   - `C:\Windows\System32\drivers\etc\hosts`
3. Add this line at the end:
   ```text
   127.0.0.1 example.com
   ```
4. Save the file.
5. Open a browser and visit `http://example.com` or `http://example.com/test_v3.html`.

### macOS / Linux

1. Open a terminal.
2. Edit the hosts file with sudo:
   ```bash
   sudo nano /etc/hosts
   ```
3. Add this line:
   ```text
   127.0.0.1 example.com
   ```
4. Save and close the editor.
5. Visit `http://example.com` or `http://example.com/test_v3.html` in your browser.

## Run the local server

This repository can be served with a simple local web server. Run the command from the repository folder.

### Python

```bash
python -m http.server 80
```

### PHP

```bash
php -S localhost:80
```

### Node.js

If you have `npm` installed:

```bash
npm install -g http-server
http-server -p 80
```

Or with `npx`:

```bash
npx http-server -p 80
```

### Go

Create a small Go file and run it:

```go
package main

import (
    "net/http"
)

func main() {
    http.ListenAndServe(":80", http.FileServer(http.Dir(".")))
}
```

Then run:

```bash
go run server.go
```

> If port 80 is already in use or requires administrative rights, use a different port such as `8080` and update the URL and hosts configuration accordingly.

## Verify reCAPTCHA token in the browser

1. Open the page in your browser:
   - `http://example.com/test_v2.html`
   - or `http://example.com/test_v3.html`
2. Open Developer Tools / Console.
3. Reload the page.
4. Look for a token printed in the console.

The page should show a reCAPTCHA response token. This is the token you can send to Google for verification.

## Verify the token with Google

Once you have the token, verify it using the reCAPTCHA verify endpoint.

Example with `curl`:

```bash
curl -d "secret=YOUR_SECRET_KEY&response=TOKEN_FROM_BROWSER" \
  -X POST https://www.google.com/recaptcha/api/siteverify
```

Replace:
- `YOUR_SECRET_KEY` with your reCAPTCHA secret key
- `TOKEN_FROM_BROWSER` with the token shown in the browser console

If successful, Google returns a JSON response with `success: true` and score or action data depending on your reCAPTCHA version.

## Notes

- Use a valid Google reCAPTCHA site key and secret key.
- `test_v2.html` and `test_v3.html` are intended to demonstrate how to trigger and inspect the token.
- Make sure your browser is allowed to access `example.com` after editing the hosts file.
# recaptcha-testing
