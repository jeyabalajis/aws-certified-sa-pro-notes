# Cognito

## Cognito User Pools
- Tokens can be exchanged for AWS access via Amazon Cognito identity pools.
- Supports user federation through a third-party identity provider. 

## Cognito Identity Pools
- Supports anonymous guest users

## User Pools vs Identity Pools

> User pools are for authentication (identity verification). With a user pool, your app users can sign in through the user pool or federate through a third-party identity provider (IdP).

> Identity pools are for authorization (access control). You can use identity pools to create unique identities for users and give them access to other AWS services.

For example, if each mobile app user needs specific temp credentials to access aws services, then it is useful.

## References

https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-scenarios.html#scenario-identity-pool
