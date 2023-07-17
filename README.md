

# Broadcasting

To enable broadcasting in your Laravel and React application, follow these steps:

1. Copy the `.env.example` file and rename it to `.env`. Make sure the necessary broadcasting configurations are properly set in the `.env` file.

2. Start the Laravel development server by running the following command:
php artisan serve

3. Start the Laravel Echo Server by running the following command:
laravel-echo-server start
git 
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







