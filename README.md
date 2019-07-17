# Lumen Login Throttling
Laravel Login Throttling for Lumen framework

## Installing

Require this package, with [Composer](https://getcomposer.org/), in the root directory of your project.

```
composer require sanchescom/lumen-throttles-logins
```

## Usage

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Sanchescom\Foundation\Auth\AuthenticatesUsers;
use Laravel\Lumen\Routing\Controller;

class AuthController extends Controller
{
    use AuthenticatesUsers;

    /** @var int */
    protected $maxAttempts = 3;

    /** @var int */
    protected $decayMinutes = 5;
    
    /**
     * Handle a login request to the application.
     *
     * @param  \Illuminate\Http\Request $request
     *
     * @throws \Illuminate\Validation\ValidationException
     *
     * @return mixed
     */
    public function login(Request $request)
    {
        $this->validateLogin($request);

        if ($this->hasTooManyLoginAttempts($request)) {
            $this->fireLockoutEvent($request);

            return $this->sendLockoutResponse($request);
        }

        if ($this->attemptLogin($request)) {
            return $this->sendLoginResponse($request);
        }
        
        $this->incrementLoginAttempts($request);

        return $this->sendFailedLoginResponse($request);
    }
}
```

[Laravel Login Throttling documentation](https://laravel.com/docs/5.8/authentication#login-throttling)

