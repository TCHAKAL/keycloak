# keycloak
Demo for Spring boot &amp; Keylock

First of all you nedd an envirnement of a keycloak for tha we will need a docker image
  Docker cmd for Keyclock : docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:22.0.0 start-dev
  Keyclock will be started at port 8080 ! 

Next Steps configure our keyCloak envirnement :

- First : open your Keycloak Admin Console at : http://localhost:8080 (Login : admin | password : admin by default)
- Create Realm for your app , in my case RGPD
- Create a new client, in my case rgpd-rest-api (if you have a deferent name you have to change it in keylock.yml file)
 the in the login tab set the Root, Home and redirect urls for your app (http://localhost:8081 and http://localhost:8081/* in my case)
- In tab Roles of your client (Clients -> Client details), create roles of your application (2 roles in my case client_admin and client_user)
- In Realm roles we need to create 2 realm roles admin and client, after that we need to asociate our roles client_admin and client_user for each of them
- Last step we will create our users in Users in my case i have 2 users : tchakal with  client_admin role and user with  client_user role


Last thing we will use postman to test our keylock config and our DemoController

1- Keycloak auth :
  Endpoint : 
            #POST  http://localhost:8080/realms/RGPD/protocol/openid-connect/token
  Body :  grant_type = password
          client_id = rgpd-rest-api
          username = tchakal |  username = client
          password = tchakal |  password = user
  Response : {
              "access_token" : "eyJhbGciOiJ ......",
               "expires_in": 300,
               "refresh_expires_in": 1800,
               "refresh_token": "eyJhbGciOiJS....",
               "token_type": "Bearer",
               "not-before-policy": 0,
               "session_state": "2a599ee0-4ff6-42e3-83be-af196096f6b6",
               "scope": "profile email"
           }
           
2- Demo controller : 
Endpoints : 
           #GET http://localhost:8082/api/v1/demo for client_user test
           #GET http://localhost:8082/api/v1/demo/hello2 for client_admin test
 Body :  BearerToken : "eyJhbGciOiJ ......"
 Responses: 
          client_user :  HELLO FROM SPRING BOOT & KEYCLOAK
                         403
          client_admin : 403
                         HELLO FROM SPRING BOOT & KEYCLOAK - ADMIN
