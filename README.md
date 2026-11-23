# Maestro Mobile Test Suite — Sauce Labs Demo App

Automated mobile UI tests using [Maestro](https://maestro.mobile.dev/) against the [Sauce Labs My Demo App](https://github.com/saucelabs/my-demo-app-rn) (React Native).

Tests run automatically on GitHub Actions using a cloud Android emulator — no local setup required.

---

## Test Coverage

### Smoke Suite (`smoke`)
Runs on every push and pull request.

| Flow | Journey |
|------|---------|
| `login.yaml` | Valid login → Products screen visible |
| `checkout.yaml` | Login → Add to cart → Checkout → Order confirmed |
| `logout.yaml` | Login → Menu → Log Out → Back to login screen |

### Regression Suite (`regression`)
Runs after smoke suite passes.

| Flow | Journey |
|------|---------|
| `login-error.yaml` | Invalid credentials → Error message shown |
| `product-list.yaml` | Scroll full product list → Open product detail |
| `sort-products.yaml` | Sort by name descending → Sort by price ascending |

---

## Project Structure

```
.maestro/
  subflows/
    login.yaml                  # reusable login setup
    dismiss-permissions.yaml    # handles OS permission dialogs
    add-to-cart.yaml            # adds first item to cart
  flows/
    smoke/
      login.yaml
      checkout.yaml
      logout.yaml
    regression/
      login-error.yaml
      product-list.yaml
      sort-products.yaml
.github/
  workflows/
    maestro.yml                 # CI pipeline
```

---

## How It Works

```
Push to GitHub
     ↓
GitHub Actions spins up Ubuntu runner
     ↓
Downloads latest Sauce Labs Demo APK from GitHub Releases
     ↓
Starts Android emulator (Pixel 6, API 34)
     ↓
Installs APK → Runs Maestro flows
     ↓
Uploads JUnit report + screenshots as artifacts
```

No Android Studio, no local emulator, no Java installation needed on your machine.

---

## Test Credentials

The Sauce Labs Demo App ships with built-in test accounts:

| Username | Password |
|----------|----------|
| `bob@example.com` | `10203040` |

---

## Tech Stack

- **Maestro** — mobile UI test framework (YAML-based flows)
- **Sauce Labs My Demo App** — open-source React Native shopping app
- **GitHub Actions** — CI/CD pipeline
- **reactivecircus/android-emulator-runner** — cloud Android emulator
