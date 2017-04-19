[auth](https://docs.djangoproject.com/en/1.10/topics/auth/default/)
[custom auth](https://docs.djangoproject.com/en/1.10/topics/auth/customizing/)
# User Authentication Django
> handles both authentication and authorization

### Auth system consists of:
	* user
	* Permissions
	* Groups
	* Configurable password hashing system
	* Forms and views tools for login and restricting content
	* Pluggable 
- third-part pckg: password strength check, throttling of login attempts, authentication of against third-parties

## Installation

## Authenticating User : (login)
- `authenticate()` : verify set of credentials
- `user = authenticate(username=username, password=password)`
