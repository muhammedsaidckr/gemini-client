# Gemini PHP API Client

A community-maintained PHP API client that allows you to interact with the Google Gemini AI API.

## Project Overview

- **Main Technologies:** PHP 8.1+, PSR-18 (HTTP Client), PSR-17 (HTTP Factories).
- **Architecture:** 
    - **Client:** The main entry point (`Gemini\Client`) manages resources.
    - **Resources:** Specialized classes for different API features (`ChatSession`, `GenerativeModel`, `EmbeddingModel`, `Models`).
    - **Data:** DTOs for structured data exchange (`Content`, `Blob`, `GenerationConfig`, etc.).
    - **Transporters:** Handles the underlying HTTP communication via PSR-18 clients.
    - **Requests/Responses:** Dedicated classes for API request payloads and response parsing.
    - **Testing:** Built-in fakes and helpers for robust testing.

## Building and Running

### Prerequisites
- PHP 8.1 or higher.
- Composer.

### Key Commands
- `composer install`: Install all project dependencies.
- `composer test`: Run the full suite of checks (linting, static analysis, and unit tests).
- `composer test:unit`: Run only the unit tests using [Pest](https://pestphp.com/).
- `composer test:types`: Run [PHPStan](https://phpstan.org/) for static type analysis.
- `composer test:lint`: Check code style without making changes.
- `composer lint`: Automatically fix code style issues using [Laravel Pint](https://laravel.com/docs/pint).

## Development Conventions

- **Strict Typing:** All files should use `declare(strict_types=1);`.
- **PSR Standards:** Follows PSR-4 for autoloading and uses PSR-18/PSR-17 for HTTP interoperability.
- **Resource Pattern:** New API features should be implemented as separate resources in `src/Resources/` and exposed via `Gemini\Client`.
- **Data Integrity:** Use DTOs in `src/Data/` and Enums in `src/Enums/` for all structured API data.
- **Testing:** 
    - Write tests using Pest in the `tests/` directory.
    - Use `Gemini\Testing\ClientFake` for mocking API responses in integration-like tests.
    - Use the `mockClient()` and `mockStreamClient()` helpers in `tests/Pest.php` for low-level transporter mocking.
- **Contributions:** Ensure all tests and linting pass (`composer test`) before submitting a PR.
