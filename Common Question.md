# how JWT token works ?



    Let's take example of 'Client' (A) and 'Server' (B).

    The Client(A) wants to access some protected data on our Server(B), but the Server knows Clients cannot be trusted.

    The Server only wants to give the data to a trustworthy Client. So our Client sends a request to our Server along with data to verify who it is.

    Instead of saving the user data, the Server instead creates a token and this token is returned to the Client and it's up to the Client to store this data, and send it along as required for any requests to the Server.

    The next time our Client makes a request along a secure route, It sends along the job token.

    But our Server knows not to trust the Client, So our Server verifies this token is who it says it's from and that it hasn't been tampered with.

    If everything checks out to be OK, the Server sends back a response with the requested data. That's it !!.




# 1 ) Difference between authentication and authorization

  A ) Authentication  = means varify the identity of user or service <br/>
  B ) Authorization   = Means determines their access rights
  
  
# 2 ) What do you mean by REST API

    it is a application programming interface that conform to the constraint of rest architectural style and allows for information with rest full web 
    services
    
# 3 ) why is REST API is used
    
    it is provide a grate flexibility data
    
# 4 ) What is difference between API and REST API

    1 ) API = The primary goal of API is to standardize data exchange between web service . Depending on the Type of API <br/>
    2 ) Rest API = is an architectural style for building web serivices that interact via an HTTP protocol
    
 # 5 ) Is REST API a framework ?
 
    The REST api is part of the integration framework and handel request from external consumers . when an external consumer initiates a request. 
    a REST API controller direct the resource request to the appropriates resource handler .
    
  # 6 ) What is integration framework
    The integration framework architecture includes the data, transport, communication, and security components required to exchange information <br/>
    between separate applications and systems
    
 # 7 ) What is rest api and how it work
  
      A RESTFUl API is an architectural style for an application program interface (API ) that iuses HTTP reqeusts to access and use data. <br/>
      That data can be used to GET , PUT , POST and DELETE data type which referes to the readign , updating , creating and delete of operations conerning<br/>
      resource
      
  # 8 ) What is difference between REST and web API
  
      web api are lightweight architecture , they are designed for gadgets constrained to devices like smartphones . 
      
  # 9 ) what are the rest method 
  
       1 ) GET  = Retrive information about the REST API resource <br/>
       2 ) POST = create a REST API resource<br/>
       3 ) PUT  = update a rest API resource<br/>
       4 ) DELETE = Delete REST api resource or related componet<br/>
       
  # 10 ) what is serializer in Django
  
        DRF is responsible for converting objects into data types understandable by javascript and front end framework
        
  # 11 ) what doees a serializer do
        
          serializers allow complex data such as querysets and model instances to be converted to native python datatypes that can be easily <br/>
          rendered into JSON , XML or other content types
          
   # 12 ) what is slug in django
          
          A slug is a short label for something , containig only latters , numbers , underscores or hyphens
          
   # 13 ) what is jwt token
   
          JWT, short for JSON Web Token is an open standard for communicating authorization details between server and client.
          
   # 14 ) what is jinja template
   
        It is a text-based template language and thus can be used to generate any markup as well as source code<br/>
        . The Jinja template engine allows customization of tags, filters, tests, and globals. Also, unlike the<br/>
        Django template engine, Jinja allows the template designer to call functions with arguments on objects
        
   # 15 ) what is coockes
        
         A cookie is a small piece of information which is stored in the client browser.It is used to store user's <br/>
         data in a file permanently (or for the specified time) . Cookie has its expiry date and time and removes <br/>
         automatically when gets expire. Django provides built-in methods to set and fetch cookie.
      
