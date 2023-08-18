# Start a new project

When you run the command:

```
composer create-project --prefer-dist laravel/laravel book-review
```

Composer will create a new Laravel project named "book-review" by downloading the Laravel framework package and its dependencies in a pre-compiled (distribution) format for faster installation. The end result will be a ready-to-use Laravel project structure within the "book-review" directory.

The command "composer create-project" is used in the context of PHP package management using Composer. It allows you to create a new project based on a specific package or framework. In this case, you are using it to create a new Laravel project with the name "book-review". Let's break down the options used in the command:

1. `--prefer-dist`: This option tells Composer to prefer downloading and installing distribution archives (zip or tar) of packages, if available. This can lead to faster installation times compared to the `--prefer-source` option, which clones the packages' source code repositories. Using `--prefer-dist` is the recommended choice for production environments where you primarily want the compiled and optimized code.

2. `laravel/laravel`: This is the package name that you want to use as the basis for your new project. In this case, it's the official Laravel framework package. Composer will download and install the specified package along with its dependencies to set up the new project.

3. `book-review`: This is the name of the directory where the new project will be created. Composer will create a directory named "book-review" and set up the Laravel project structure within it.

## Setup the database in `.env`
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel-book-review
DB_USERNAME=root
DB_PASSWORD=<secret-password> # This is the only thing I need to change
```

## Create models
```
php artisan make:model Book -m
php artisan make:model Review -m
```
The command "php artisan make:model" is used in Laravel, a PHP web application framework, to generate a new Eloquent model class. Eloquent is Laravel's built-in ORM (Object-Relational Mapping) system that allows you to interact with your database using PHP classes and objects. The command takes various options and arguments to customize the model creation process. In your specific example, you're using the following command:

```
php artisan make:model Book -m
```

Let's break down the options and arguments:

1. `make:model`: This part of the command is indicating that you want to create a new Eloquent model class.

2. `Book`: This is the name of the model you're creating. In this case, it's named "Book". Model names in Laravel are typically singular and follow the PascalCase naming convention.

3. `-m`: This option tells Laravel to also create a corresponding database migration file along with the model. Migrations in Laravel are used to define and manage changes to your database schema. By including this option, Laravel will generate a migration file that creates the necessary table for the "Book" model in the database.

So, when you run the command "php artisan make:model Book -m", Laravel will create two things:

1. A new Eloquent model class named "Book" in the "app" directory. This class can be used to interact with the "books" table in the database, following Laravel's conventions.

2. A new database migration file in the "database/migrations" directory. This migration file will contain the schema definition for creating the "books" table in the database. You can customize this schema according to your needs, specifying the columns and their data types.

After running the command and making any necessary adjustments to the migration file, you would use Laravel's migration system to apply the changes to the database using the `php artisan migrate` command. This will create the "books" table in your database, allowing you to start using the "Book" model to interact with the table's data using Eloquent.

Update the `books` table in the database:
```php
return new class extends Migration {
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('books', function (Blueprint $table) {
            $table->id();
            // Add entries to DB
            $table->string('title');
            $table->string('author');
            // End of entries
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('books');
    }
};
```

and the `reviews` table:
```php
return new class extends Migration {
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('reviews', function (Blueprint $table) {
            $table->id();
            // Add entries to DB
            $table->text('review');
            $table->unsignedTinyInteger('rating');
            // End of entries

            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('reviews');
    }
};
```