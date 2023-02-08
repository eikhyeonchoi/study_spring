# ExceptionsAndErrors
```
TransientPropertyValueException
Thrown when a property cannot be persisted because it is an association with a transient unsaved entity instance.
-> fk 없을 때 오류
```
```
no serializer found for class org.hibernate.proxy.pojo.bytebuddy.bytebuddyinterceptor and no properties discovered to create beanserializer
: LAZY 때문에 생기는 오류
: return 할 때 dto를 사용하자
```