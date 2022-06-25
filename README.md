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
    
               property name —> camelCase, e.g., orderId.
    
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

### Response Data Object

controller response is always named in **camel case**

For Example, this is correct:

```jsx
const submitpayoutsdetails = catchAsync(async (req, res) => {
const AuthData = await authService.getAuthById(req.SubjectId);
const payOutDetails = await doctorprofileService.submitpayoutsdetails(req.body, AuthData);
res.status(httpStatus.CREATED).json({
message: 'Payouts Details Submitted!',
payOutDetails: payOutDetails,
});
});
```

But this is NOT correct:

```jsx
const fetchpayoutsdetails = catchAsync(async (req, res) => {
const AuthData = await authService.getAuthById(req.SubjectId);
const payoutData = await doctorprofileService.fetchpayoutsdetails(AuthData);
if (!payoutData) {
throw new ApiError(httpStatus.BAD_REQUEST, 'Your OnBoarding is pending data submit');
} else {
res.status(httpStatus.OK).json({ 'Payout Details': payoutData });
}
```

### Design patterns Notes:

> Try to keep routes and controllers minimal.
For example: instead of having 2 routes:
route.get("get-appointments") and router.post("post-appointment")
One is enough route.get("appointments") and router.post("appointments")
> 

> methods should start with a verb:
Example: const upcomingAppointments --change to--> getUpcomingAppointments
> 

> fetch or get in naming??
get is preferable. For example: getUserDetails NOT fetchUserDetails
> 

To Discuss: What do you think about assigning userObject with each request ???

Always keep userId (the user who is requesting an API) in the models and send it with the requests.

Always start your controller with calling authentication service and get requester authentication data:

Something like:

```jsx
const AuthData = await authService.getAuthById(req.SubjectId);
```

```jsx
In Mongoose: 
use find and save or create and save instead of findandUpdate
apply restrictions like:
age {
	min:1,
	max: 100,
	validate: { xxxxx }
},
email {
	minlength: 10,
	lowercase: true,	
},
```

[https://www.youtube.com/watch?v=D4Dja5WSZoA](https://www.youtube.com/watch?v=D4Dja5WSZoA)

Delete and Update request should return the updated data.
