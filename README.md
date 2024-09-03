# Blackfire Labeler Task Runner

## Overview

This repository provides a utility to facilitate the labeling of transactions using Blackfire and manage their associated data in Redis. The primary components include a task runner, a Blackfire labeler, and a Redis configuration class. The setup is intentionally small and siloed to handle only the limited functionality required for Blackfire profiling and Redis data management. This design is deliberate because the code is intended to be called from an `auto_prepend` file. By keeping it isolated, we avoid potential conflicts with the `RedisConfig` class or other configurations used in different parts of the project or other projects.

## Project Structure

```plaintext
.
├── src
│   ├── ItJonction
│   │   └── BlackfireLabeler
│   │       ├── BlackfireLabelerController.php
│   │       ├── CommonTaskRunner.php
│   │       ├── BlackfireLabeler.php
│   │       └── RedisConfig.php
├── tests
│   └── BlackfireIntegration
│       └── BlackfireTest.php
├── build
│   └── logs
│   └── coverage-html
├── redisValues.php.example
├── composer.json
├── phpunit.xml
└── README.md
```

### Installation

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/IT-JONCTION/blackfire-labeler.git
   cd blackfire-labeler
   ```

2. **Install Composer Dependencies:**

   ```bash
   composer install
   ```

3. **Set Up Redis Configuration:**

   The repository includes a `redisValues.php.example` file, which serves as a template for your Redis configuration. To set up your environment:

   1. Copy the example file to `redisValues.php`:

      ```bash
      cp redisValues.php.example redisValues.php
      ```

   2. Open the newly created `redisValues.php` file in a text editor and configure your Redis connection settings:

      ```php
      <?php
      $scopedRedisEnvValues = [
          'REDIS_HOST' => '127.0.0.1',
          'REDIS_PORT' => 6379,
          'REDIS_USERNAME' => '',
          'REDIS_PASSWORD' => '',
          'REDIS_DB' => 0,
          'REDIS_SECURE' => false,
      ];
      ```

   > **Note:** `redisValues.php` should contain your actual configuration values and should **not** be committed to version control. The `.gitignore` file has been configured to exclude this file.

4. **Running Tests Locally:**

   If you want to run the tests locally, make sure you have a Redis instance running. You can either install Redis directly on your machine or use Docker. For simplicity we have assumed that you have Redis and PHP 8.2 installed. Feel free to create an issue requesting docker, or better still add a PR.

   - **Using Local Redis Installation**:
     1. Start your local Redis server:
        ```bash
        redis-server
        ```
     2. Run PHPUnit tests:
        ```bash
        ./vendor/bin/phpunit --configuration phpunit.xml
        ```

## Usage

### Configuring the Auto Prepend File

To enable Blackfire profiling and Redis data management for your PHP application, you need to reference the `BlackfireLabelerController.php` file in the `auto_prepend_file` directive of your `php.ini` configuration.

Add the following line to your live `php.ini` file:

```ini
auto_prepend_file = /your/path/to/blackfire-labeler/ItJonction/BlackfireLabeler/BlackfireLabelerController.php
```

This setup ensures that the Blackfire labeling and Redis logging tasks are automatically executed at the start of each request, before any other PHP scripts run.

## Customizing Redis Configuration

- The `redisValues.php` file contains all the necessary configuration values for connecting to your Redis instance. This includes options for the host, port, authentication, and database selection. Remember to update this file according to your environment.

## Logging and Profiling

- **BlackfireLabeler** offers methods for:
  - Generating unique request hashes.
  - Logging request details and included files to Redis.
  - Setting transaction names for Blackfire profiling.
  - Archiving and managing logs.

## Contributing

Contributions to this repository are welcome. Please fork the repository and create a pull request for any feature enhancements or bug fixes. Ensure that your code adheres to the project's coding standards and includes appropriate tests.

## License

This project is licensed under the MIT License. See the `LICENSE` file for more details.

## Support

If you encounter any issues or have questions, please open an issue in the repository or contact the maintainers.
