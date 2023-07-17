<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.

## Learning Laravel

Laravel has the most extensive and thorough [documentation](https://laravel.com/docs) and video tutorial library of all modern web application frameworks, making it a breeze to get started with the framework.

If you don't feel like reading, [Laracasts](https://laracasts.com) can help. Laracasts contains over 1500 video tutorials on a range of topics including Laravel, modern PHP, unit testing, and JavaScript. Boost your skills by digging into our comprehensive video library.

## Laravel Sponsors

We would like to extend our thanks to the following sponsors for funding Laravel development. If you are interested in becoming a sponsor, please visit the Laravel [Patreon page](https://patreon.com/taylorotwell).

### Premium Partners

- **[Vehikl](https://vehikl.com/)**
- **[Tighten Co.](https://tighten.co)**
- **[Kirschbaum Development Group](https://kirschbaumdevelopment.com)**
- **[64 Robots](https://64robots.com)**
- **[Cubet Techno Labs](https://cubettech.com)**
- **[Cyber-Duck](https://cyber-duck.co.uk)**
- **[Many](https://www.many.co.uk)**
- **[Webdock, Fast VPS Hosting](https://www.webdock.io/en)**
- **[DevSquad](https://devsquad.com)**
- **[Curotec](https://www.curotec.com/services/technologies/laravel/)**
- **[OP.GG](https://op.gg)**
- **[WebReinvent](https://webreinvent.com/?utm_source=laravel&utm_medium=github&utm_campaign=patreon-sponsors)**
- **[Lendio](https://lendio.com)**

## Contributing

Thank you for considering contributing to the Laravel framework! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).


## BROADCASTING

# Broadcasting

To enable broadcasting in your Laravel and React application, follow these steps:

1. Copy the `.env.example` file and rename it to `.env`. Make sure the necessary broadcasting configurations are properly set in the `.env` file.

2. Start the Laravel development server by running the following command:
php artisan serve

3. Start the Laravel Echo Server by running the following command:
laravel-echo-server start

4. Start the Laravel queue worker for processing jobs by running the following command:
php artisan queue:work


5. In your frontend React code, create a `useBroadcast()` hook to handle the broadcasting setup. This hook will establish the connection with Socket.IO and Laravel Echo. Here's an example of how it can be implemented:

```javascript
import React, { useEffect } from "react";
import Echo from "laravel-echo";
import io from "socket.io-client";
import { AUTH_COOKIE } from "constants/constants";

declare global {
  interface Window {
    Echo: Echo;
    io: any;
  }
}

export const useBroadcast = () => {
  useEffect(() => {
    const cookie = `Bearer ${Cookies.get(AUTH_COOKIE)}`;

    if (!cookie) return;

    const echo = new Echo({
      broadcaster: "socket.io",
      host: "http://localhost:6001",
      client: io,
      authEndpoint: "/broadcasting/auth",
    });

    window.Echo = echo;
  }, []);
};

You can also create another custom hook, such as useBroadcastMessageTest(), to handle specific event listeners and notifications. Here's an example:

import { useEffect, useState } from "react";
import { NotificationManager } from "react-notifications";

export const useBroadcastMessageTest = () => {
  const [messageUpdate, setMessageUpdate] = useState(false);

  const messageEventCallback = (data) => {
    console.log("Broadcast received:", data);
    setMessageUpdate(data);
    NotificationManager.info("New message", "Check the message view.");
  };

  useEffect(() => {
    window.Echo.channel("message").listen(".message", messageEventCallback);

    // Clean up the event listener if necessary
    // return () => {
    //   window.Echo.leaveChannel("message");
    // };

    // eslint-disable-next-line
  }, []);

  return messageUpdate;
};


Finally, you can use the custom hooks in the desired component:

import React, { FC } from "react";
import { useBroadcastMessageTest } from "@hooks/useBroadcastMessageTest";

interface Props {
  children: JSX.Element;
  icon?: JSX.Element;
  title: string;
}

const SidebarLayout: FC<Props> = ({ children, icon, title = "" }) => {
  const newMessages = useBroadcastMessageTest();

  return (
    <>
      {/* Your component code */}
      {/* Access the new messages via `newMessages` */}
    </>
  );
};

export { SidebarLayout };


By following these steps and utilizing the custom hooks, you can establish the broadcasting setup between your Laravel backend and React frontend, enabling real-time communication and event handling.







