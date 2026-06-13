# Django Rest Frame Work 

Client -> url -> View  -> Serializers -> Model -> Database

Response return back : 

Database -> Model -> Serializers -> View -> Response -> Client


Layer 1 : Model : Database Strcture

Layer 2: Serializer : Convert Python Object to JSON

Responsibility :  Validation, Transformation, Serialization, Deserialization 

Layer 3: Views: Business Logic

Layer 4: urls: maps endpoint to business logic. Request flow : [Post Request, url, view, serializer validation, model, database]

Core DRF Components: 
         API 