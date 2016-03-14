---
published: true
title: Struts Login With Internationalization and Localization Demo
layout: post
---
#Struts Login With Internationalization and Localization Demo

This is a very simple login demo web-app in struts 1.3 which support Internationalization and Localization (or a multilingual application which in *English, Hindi, German, French and Spanish* ). So before going any forward I have something share few day before writing this post I attended an interview for Struts 1x I have around 2+ year of experience in struts 1 and then ask me how to handle [Internationalization and Localization](https://en.wikipedia.org/wiki/Internationalization_and_localization) in struts. And I have never worked with multilingual application before but have some idea about multiple resource file ending with a locale so as I came back got some time wrote and demo for the multilingual app.
You can download [source from here](https://github.com/digvijaybhakuni/demo-struts1x/archive/multiligual-done.zip)

![Struts Internationalization Screenshot by DGStack.com](https://docs.google.com/drawings/d/1VxfqC68z_qd7uU3bXVDQd_oC8LzCc0tfFvF3UXg_NUM/pub?w=500&h=350)

>[TOC]

####Project Structure
This project structure is maven-archetype-webapp. And depencencies use are listed below. 
>- struts-core 1.3.10
>- struts-taglib 1.3.10
>- junit 3.8.1

![Project Structure multilingual DGStack.com](https://docs.google.com/drawings/d/1sCpy0y4qApPRj_7rSMAsFzhT3NHq9Ca47cNQs1XWcBU/pub?w=449&h=589)

####pom.xml
```xml
	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>

		<!-- Struts core lib -->
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts-core</artifactId>
			<version>1.3.10</version>
		</dependency>

		<!-- Struts tag lib for jsp -->
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts-taglib</artifactId>
			<version>1.3.10</version>
		</dependency>
	</dependencies>
```
We have used  5 properties file for five languages. 
>- ApplicationResource_de.properties
>- ApplicationResource_es.properties
>- ApplicationResource_fr.properties
>- ApplicationResource_hi.properties
>- ApplicationResource.properties

####ApplicationResource_de.properties
```txt

dgstack.header.login=Melde dich an, um Anwendungs

dgstack.label.login.username=Benutzername

dgstack.label.login.password=Passwort

dgstack.label.login.submit=Einloggen

dgstack.label.login.reset=rücksetzen

dgstack.label.global.change.language=Sprache Ändern

dgstack.error.username.empty=Benutzername sollte nicht leer
dgstack.error.password.empty=Kennwort sollte nicht leer
dgstack.error.login.fail=Ungültiger Benutzername oder Passwort

dgstack.header.success=Anmeldung erfolgreich,


errors.header=<ul>
errors.footer=</ul>
errors.prefix=<li>
errors.suffix=</li>
```
Similar to above properties file. We have all the other properties files.

####index.jsp
This jsp is the first thing to appear on the screen here one can login or can language from the bottom. To show all the label as per our selected language we have used **bean:message** tag to display text from properties file corresponding to there key.
```html
<html:form action="/login" method="post" focus="username">
<table>
	<tr>
		<td colspan="2" style="color:red;">
			<html:errors />
			<logic:equal value="true" name="loginError">
				<bean:message key="dgstack.error.login.fail"/>
			</logic:equal>
		</td>
	</tr>
	<tr>
		<td><bean:message key="dgstack.label.login.username"/></td>
		<td><html:text property="username"></html:text></td>
	</tr>
	<tr>
		<td><bean:message key="dgstack.label.login.password"/></td>
		<td><html:password property="password"></html:password></td>
	</tr>
	<tr>
		<td><html:submit><bean:message key="dgstack.label.login.submit"/></html:submit></td>
		<td><html:reset><bean:message key="dgstack.label.login.reset"/></html:reset></td>
	</tr>
</table>
<div>
	<h5><bean:message key="dgstack.label.global.change.language"/></h5>
	<a href='changeLang.do?to=en'>English</a> | 
	<a href='changeLang.do?to=hi'>&#2361;&#2367;&#2306;&#2342;&#2368;</a> | 
	<a href='changeLang.do?to=de'>Deutsche</a> |
	<a href='changeLang.do?to=fr'>Français</a> | 
	<a href='changeLang.do?to=es'>Español</a>  
</div>
</html:form>
```
####ChangeLanguageAction.java
This Action class is use to change Locale of the current session context. As you can see in the following code we setting a **Globals.LOCALE_KEY** in session to as per our selected language. Whatever parameter value you set in *message-resources* in **struts-config.xml** it will now pick the value and append '_' and your locale, then it will look for that properties file. If that files is not founded it will load default file (means file without locale). 
In our case if we have selected french it will look for  "*ApplicationResource_fr.properties*" file.
```java
public ActionForward execute(ActionMapping mapping, ActionForm form, HttpServletRequest request,
		HttpServletResponse response) throws Exception {
	
	if("hi".equals(request.getParameter("to"))){
		request.getSession().setAttribute(Globals.LOCALE_KEY, new  Locale("hi"));//Locale for hindi is not present by default so we to add one for hindi.
	}else if("fr".equals(request.getParameter("to"))){
		request.getSession().setAttribute(Globals.LOCALE_KEY,Locale.FRENCH);
	}else if("en".equals(request.getParameter("to"))){
		request.getSession().setAttribute(Globals.LOCALE_KEY,Locale.ENGLISH);
	}else if("es".equals(request.getParameter("to"))){
		request.getSession().setAttribute(Globals.LOCALE_KEY,new  Locale("es"));//Locale for spanish is not present by default so we to add one for spanish.
	}else if("de".equals(request.getParameter("to"))){
		request.getSession().setAttribute(Globals.LOCALE_KEY,Locale.GERMAN);
	} 
	
	return mapping.getInputForward();
}
```
####struts-config.xml
This is very simple struts configuration file nothing much in it. we just have on form bean **loginform** , a **LoginAction** and  **ChangeLanguageAction** that is used to change the language.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts-config PUBLIC 
"-//Apache Software Foundation//DTD Struts Configuration 1.3//EN" 
"http://jakarta.apache.org/struts/dtds/struts-config_1_3.dtd">

<struts-config>
	
	<form-beans>
		<form-bean name="loginform" type="com.dgstack.dev.struts.demo.forms.LoginForm"></form-bean>
	</form-beans>
	
	<action-mappings>
		
		<action path="/login" name="loginform" validate="true" type="com.dgstack.dev.struts.demo.action.LoginAction" scope="request" input="/index.jsp">
			<forward name="success" path="/success.jsp" redirect="true"/>
		</action>
		
		<action path="/changeLang" type="com.dgstack.dev.struts.demo.action.ChangeLanguageAction" scope="request" input="/index.jsp">
		</action>
		
	</action-mappings>
	
	<message-resources parameter="ApplicationResource"></message-resources>
</struts-config>
```

>####If you have any question can write it down in comment below. 
> You and find the source on [github](https://github.com/digvijaybhakuni/demo-struts1x/tree/multiligual-done) or you can download it from [here](https://github.com/digvijaybhakuni/demo-struts1x/archive/multiligual-done.zip). I am sure any one can under this by just take look at complate source code.
>