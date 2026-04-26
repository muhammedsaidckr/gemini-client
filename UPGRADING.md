# Upgrading from 1.x to 2.x

This guide provides instructions for migrating dependent packages and applications from Gemini PHP Client 1.x to 2.x.

## 1. Update Dependency

Update your `composer.json` to require version `^2.0`:

```bash
composer require google-gemini-php/client:^2.0
```

## 2. API Version Change

Version 2.0 now defaults to the Gemini **v1beta** API. This enables many new features but may have subtle differences in behavior compared to the v1 API.

If you previously customized the base URL to point to `v1`, ensure it now points to `v1beta` if you are using the factory:

```php
$client = Gemini::factory()
    ->withBaseUrl('https://generativelanguage.googleapis.com/v1beta')
    ->make();
```

## 3. Deprecated Model Handling

### `ModelType` Enum
The `Gemini\Enums\ModelType` enum is deprecated and will be removed in the next major version.

**Before:**
```php
use Gemini\Enums\ModelType;

$client->generativeModel(ModelType::GEMINI_PRO);
```

**After:**
Pass the model name as a string or use your own `BackedEnum`:
```php
$client->generativeModel('gemini-2.0-flash');
```

### Specialized Client Methods
The shortcut methods for specific models have been deprecated.

**Deprecated Methods:**
- `$client->geminiPro()`
- `$client->geminiFlash()`

**Migration:**
Use the generic `generativeModel()` method instead:
```php
// Before
$client->geminiPro();

// After
$client->generativeModel('gemini-1.5-pro');
```

## 4. Method Signature Changes

Several methods that previously required `ModelType` now accept `BackedEnum|string`.

- `generativeModel(BackedEnum|string $model)`
- `generativeModelWithSystemInstruction(BackedEnum|string $model, ...)`
- `embeddingModel(BackedEnum|string $model)`
- `chat(BackedEnum|string $model)`

## 5. New Features in 2.0

Dependent packages can now leverage the following new capabilities:

- **Structured Output:** Constrain model responses to JSON or specific schemas.
- **System Instructions:** Set model behavior at the resource level.
- **File Management:** Upload and manage files (up to 2GB) for use in prompts.
- **Function Calling & Code Execution:** Enable the model to interact with external tools or run code.
- **Grounding:** Integrated Google Search and Google Maps grounding.
- **Context Caching:** Reduce latency and cost for large repetitive contexts.
- **Thinking Mode:** Support for models that expose their reasoning process (e.g., Gemini 2.0 Flash Thinking).
- **Speech Generation:** Generate audio responses directly from the model.

## 6. Testing

If you are using `ClientFake` in your tests, ensure you update any assertions that relied on the deprecated `geminiPro()` or `geminiFlash()` methods to use `generativeModel()` assertions.

```php
// Before
$fake->geminiPro()->assertSent(...);

// After
$fake->generativeModel('gemini-1.5-pro')->assertSent(...);
```
