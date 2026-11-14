# GameVault Assignment — Login System Test Cases

## Test assumptions
- Sample valid account: username `user1`, password `pass123`.
- Password length is measured in characters; exactly 6 characters satisfies the minimum.
- Failed authentication attempts are counted only when both fields pass input validation and credentials are rejected.
- Lockout tests start with a fresh account or reset test environment.
- No lockout duration is specified, so the expected result is simply that the account remains locked.

| Test Case ID | Test Scenario | Input Data | Expected Output |
|---|---|---|---|
| TC_01 | Submit with an empty username | Username: empty; Password: `pass123`; click Login | Do not submit. Display “Username is required.” Do not add a failed-login attempt. |
| TC_02 | Submit with an empty password | Username: `user1`; Password: empty; click Login | Do not submit. Display “Password is required.” Do not add a failed-login attempt. |
| TC_03 | Password shorter than 6 characters | Username: `user1`; Password: `abc12` (5 characters); click Login | Do not submit. Display “Password must be at least 6 characters.” |
| TC_04 | Password exactly 6 characters | Username: `user1`; Password: `abc123` (6 characters); click Login | Length validation passes. Since credentials are not the sample valid pair, display “Invalid username or password.” Count one failed attempt. |
| TC_05 | Login with valid credentials | Username: `user1`; Password: `pass123`; click Login | Display “Login successful.” Allow access to the authenticated/home page. |
| TC_06 | Login with an incorrect password | Username: `user1`; Password: `wrong123`; click Login (attempt 1 of 3) | Display “Invalid username or password.” Account remains unlocked; failed-attempt count becomes 1. |
| TC_07 | Third consecutive failed login attempt | Username: `user1`; Password: `wrong123`; click Login after two previous failed attempts | Display “Account locked after 3 consecutive failed login attempts.” Deny login and mark the account locked. |
| TC_08 | Try logging in after account is locked | Username: `user1`; Password: `pass123`; click Login after TC_07 | Deny login even with correct credentials. Display “Account is locked. Login is not allowed.” |

## Coverage summary
- Empty username: TC_01
- Empty password and minimum password length: TC_02–TC_04
- Successful login: TC_05
- Invalid credentials: TC_04 and TC_06
- Lockout after three consecutive failed attempts and blocking a later login: TC_07–TC_08
