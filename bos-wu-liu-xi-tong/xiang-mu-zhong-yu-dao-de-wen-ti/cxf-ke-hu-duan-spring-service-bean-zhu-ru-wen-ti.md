# cxf客户端spring service bean注入问题

## 在applicationContext使用&lt;jaxws:client...注入cxf服务无法使用@autowired获得bean的问题？

```text
<jaxws:client id="crmClient" address="http://localhost:8080/service/customer" serviceClass="com.wp.utils.crm.ICustomerService"></jaxws:client>
```

 在cxf服务中加上@Service

```text
@WebService(name = "ICustomerService", targetNamespace = "http://service.wp.com/")
@XmlSeeAlso({
})
@Service
public interface ICustomerService {

```

 





