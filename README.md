# REST_API_Design_Best_Practices

## Naming Format:

- Endpoint paths:
    
          **nouns** instead of verbs. For example: DELETE */v1/orders/:id &* POST */v1/users.*
    
     **kebab-case** instead of camelCase. For example: *email-verification, password-reset*.
    
- Controllers and services methods: **verbs** instead of nouns. For example: delete*Order*
- Variables and methods: camelCase.
- Models:
    
               model name —> UpperCamelCase,
    
               schema name —> UpperCamelCase
    
               property name —> camelCase, e.g., *orderId*.
    
- Files: *camelCaseNoun*.type.js. For example: *orders.controller.js,* *notifications.route.js*
- Request Parameters: camelCase. For example: req.body.userName

## API request format:

all body parameter names follow: camelCase.

## API response format:

- All success calls should return a  status   and message and response (containing data).
- All failed    calls should return a  status   and message.
- All failed     remote calls should throw  an APIError with a status and a message

Response with one of the common response status: httpStatus.NOT_FOUND, .OK, .CREATED, .FORBIDDEN, .UNAUTHORIZED, .NO_CONTENT, .INTERNAL_SERVER_ERROR

### In case calling remote service:

```jsx
try {
  await call
  if (success) {
      res.status(httpStatus.OK)
				 .json({
			      message: xxxxxxxxxxxxxxx,
						response,
					});
	}
	else {
		 res.status(httpStatus.OK)
				.json({
				message: xxxxxxxxxxxxxxx,
				response,
				});
	}
	catch (err) {
		throw new ApiError(httpStatus.INTERNAL_SERVER_ERROR, due to error: ${err});
	}
```

### In case calling local service:

```jsx
await call
if (call success) :
	res.status(httpStatus.OK)
		 .json({
					message: xxxxxxxxxxxxxxx,
					response,
			});
else:
	res.status(httpStatus.XXXXXXXX)
		.json({
					message: xxxxxxxxxxxxxxx
		});
```

> Don't forget to version the API like /v1/.
>
