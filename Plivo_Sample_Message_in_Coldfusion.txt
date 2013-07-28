<cfoutput>
 
#PlivoSendSMS('{dst}','Hi from Plivo')#

</cfoutput>

<!--------------------------------------------------------------------------------------------------------->
<!------------------------------------ PlivoSendSMS Function ---------------------------------------------->
<!--------------------------------------------------------------------------------------------------------->
<cffunction name="PlivoSendSMS" access="public" returntype="boolean" output="true">
	<cfargument name="dst" type="string" required="yes" />
	<cfargument name="text" type="string" required="yes" />

	<cfoutput>
	
	<!---set your account setting here--->
	<cfset _src		= '{src}'>
	<cfset _AUTH_ID	= '{AUTH_ID}'>
	<cfset _AUTH_TOKEN	= '{AUTH_TOKEN}'>
	 
	<!---build json record--->
	<cfset _RetVal = true >
	<cfset stFields = { "src" = "#_src#", "dst" = "#arguments.dst#", "text" = "#arguments.text#" }> 
     
	<!---send it--->
     <cfhttp
     method="post"
     url="https://api.plivo.com/v1/Account/#_AUTH_ID#/Message/" result="request.getSMSS" >
     <cfhttpparam type="header" name="Content-Type" value="application/json" /> 
     <cfhttpparam type="header" name="Authorization" 
     value="Basic #ToBase64("#_AUTH_ID#:#_AUTH_TOKEN#")#" /> 
     <cfhttpparam type="body" value="#serializeJSON(stFields)#"> 
     </cfhttp>

	<!---remove cfdump for production--->
	<cfdump var="#request.getSMSS#">

	</cfoutput>

	<cfreturn _RetVal />
</cffunction>