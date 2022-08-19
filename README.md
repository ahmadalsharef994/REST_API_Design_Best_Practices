# REST_API_Design_Best_Practices

## Naming Convention:

In order to provide consistent developer experience across many APIs and over a long period of time, all names used by an API should be:
simple
intuitive
consistent

- Names used in APIs should be in correct American English. For example, license (instead of licence), color (instead of colour).
- Use the same name or term for the same concept, including for concepts shared across APIs. For example: userId should be named as userId every where in the app.
- Third-party Services (API calls) of third party products should have names similar to: appointment.dyte.js, fileUpload.S3.js. Not AppointmentSession.service.js or fileUpload.js
    
    Another example: sms.2factor.js instead of sms.service.js (In other words, filename should tell us what the service do and who is the provider).
    
- very common Abbreviation of names is allowed. For example: OTP instead of oneTimePassword but now pwd instead of password or appntmnt instead of appointment. Examples:
    - config (configuration)
    - id (identifier)
    - spec (specification)
    - stats (statistics)
- Endpoint paths:
    
          **nouns** instead of verbs. For example: DELETE */v1/orders/:id &* POST */v1/users. Instead of POST /v1/orders/delete-order/:id*
    
     **kebab-case** instead of camelCase. For example: *email-verification, password-reset*.
    
- Controllers and services methods: **verbs** instead of nouns. For example: delete*Order not orderDeletion.*
- Variables and methods: camelCase.
- Models:
    
               model name —> UpperCamelCase,
    
               schema name —> UpperCamelCase
    
               property name —> camelCase, e.g., *orderId*.
    
- Files: *camelCaseNounSinglural*.type.js. For example: *order.controller.js,* *notification.route.js, doctorAppointment.controller.js.*
    
    Not appointments.controller.js, 
    
- Request Query, Parameters, and Body: either camelCase or snake_case.  In our case, camelCase:
    
    For example: req.body.userName, req.docId **not** req.Docid
    

## API request format:

As mentioned above, all body parameter names should follow: camelCase.

## API response format:

- All success calls should return a  status   and response (json) containing message and data.
- All failed    calls should return a  status   and response having message.
- All failed     remote calls should throw  an APIError with a status and a message (json response having message).
- In other words, all responses (regardless the result) should have something like:
    - `res.status(htttpsSatus.OK).json({message: ‘xxxx’, data: dataObject});`
    - This is similar to Null-Object design pattern.
    - For example, if no appointment found with appointmentId. response should be:
        - `res.status(htttpsSatus.NOT_FOUND).json({message: ‘not found’, **data**: {});`
    - if appointment found with appointmentId. response should be:
        - `res.status(htttpsSatus.OK).json({message: ‘appointment found’, **data**: appointmentData);`
        - `NOT res.status(htttpsSatus.OK).json({message: ‘appointment found’, appointmentData: appointmentData);`
    - i.e, Front-End Developer should always find response having: status + JSON(message + **data)** regardless the endpoint he called**.**

Response with one of the common response status: httpStatus.NOT_FOUND, .OK, .CREATED,  .FORBIDDEN, .UNAUTHORIZED, .NO_CONTENT, .INTERNAL_SERVER_ERROR

### In case calling remote service:

```jsx
**try** {
  await call
  if (success) {
      res.status(httpStatus.OK)
				 .json({
			      message: xxxxxxxxxxxxxxx,
						data: response,
					});
	}
	else {
		 res.status(httpStatus.OK)
				.json({
				message: xxxxxxxxxxxxxxx,
				data: response,
				});
	}
}
**catch** (err) {
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
					data: response,
			});
else:
	res.status(httpStatus.XXXXXXXX)
		.json({
					message: xxxxxxxxxxxxxxx,
					data: []
		});
```

> Don't forget to version the API like /v1/.
> 

### Response Data

Again, controller response is always named as **data**

For Example, this is correct:

```jsx
const submitpayoutsdetails = catchAsync(async (req, res) => {
const AuthData = await authService.getAuthById(req.SubjectId);
const payOutDetails = await doctorprofileService.submitpayoutsdetails(req.body, AuthData);
res.status(httpStatus.CREATED).json({
message: 'Payouts Details Submitted!',
data: payOutDetails,
});
});
```

But this isn’t correct:
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
