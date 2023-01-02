# Python DRF(Django Rest Framework)
this repo contains python rest framework content


### For Authantication 
####1. Open settings and inclue 


```
REST_FRAMEWORK = {
    # other settings...
    'DEFAULT_AUTHENTICATION_CLASSES': [],
    'DEFAULT_PERMISSION_CLASSES': [],
}
```


### DRF Get request that convert pandas query object into json or xml formate
####1. In View.py


```
from rest_framework.decorators import api_view
from rest_framework.response import Response
from app.serializers import AllPermissionsSerializer

@api_view(['GET'])
def get_all__permissions(request):
    serializer = AllPermissionsSerializer()
    if serializer.is_valid():
        return Response(serializer.data)
    return Response({"Status":403, "data":{}})

```

####2. create seralizer.py file and write seralizing code .
####2.1 . This api return dictonary datatype , Orderdict convert into dict .

```
from rest_framework import serializers
from app import models as pm_models
from app import models as lh_models

class AllPermissionsSerializer(serializers.Serializer):
    def __init__(self, instance=None, data=..., **kwargs):
        data={
            'roles':dict(pm_models.Role.objects.values_list("SK_RoleID","Role").all()),
            'modules':dict(lh_models.Module.objects.values_list("id","module_name").all()),
            'geography':dict(lh_models.Market.objects.values_list("id","market_name").all()),
            'clients':dict(lh_models.Client.objects.values_list("id","client_name").all()),
            'groups':dict(lh_models.Group.objects.values_list("id","name").all()),
        }
        super().__init__(instance, data, **kwargs)

    roles = serializers.DictField()
    modules = serializers.DictField()
    geography = serializers.DictField()
    clients = serializers.DictField()
    groups = serializers.DictField()

    
    
    

```


