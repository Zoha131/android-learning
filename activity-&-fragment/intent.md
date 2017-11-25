## Notes
1. To destroy all activity from the stack and go to the login-activity add flags to the Intent like followings:
    ```java
    Intent intent = new Intent(this, LogINActivity.class);
    intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK);
    startActivity(intent);
    ```
