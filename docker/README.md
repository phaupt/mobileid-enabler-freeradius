freeradius-mobileid docker image
================================

Lightweight and fast FreeRADIUS 3.x server Docker image with integrated Mobile ID and LDAP/Active Directory support.

## Run

To start: 
```
 $ docker run --name freeradius-mobileid -d \
   -e AP_ID=mid://dev.swisscom.ch \
   -e AP_PREFIX="Test" \
   -e CLIENT_PWD=ThisMustStaySecret \
   -e LDAP_SERVER=ldap://yourserver.com \
   -e LDAP_USERID=CN=SystemLDAP,CN=Users,DC=org,DC=acme,DC=com \
   -e LDAP_PWD=ThisMustStaySecret \
   -e LDAP_BASEDN=CN=Users,DC=org,DC=acme,DC=ch \
   -e AP_KEY=<base64 encoded key file> \ 
   -e AP_CRT=<base64 encoded crt file> \
   -p 1812:1812/udp \
   -p 1813:1813/udp \
   swisscomtds/freeradius-mobileid
```
optional environment settings:
```
   -e LDAP_UPDATE=enabled \
   -e LDAP_USER_FILTER=(&(objectclass=inetOrgPerson)(uid=%{User-Name})) \
   -e LDAP_ATTR_MOBILE=mobile \
   -e LDAP_ATTR_LANGUAGE=preferredLanguage \
   -e LDAP_ATTR_SNOFDN=msNPCallingStationID \
   -e DEFAULT_LANGUAGE=de \
   -e UNIQUEID_CHECK=ifset \
   -e ALLOWED_MCC="228,295" \
```

Infos about the `-e` settings:

* AP_ID: AP customer/client identification towards Mobile ID service
* AP_PREFIX: AP prefix that will be added to the message sent to the mobile
* CLIENT_PWD: Radius client password / shared secret
* LDAP_SERVER: Active Directory / LDAP server address in the form ldap://ipOrDNS
* LDAP_USERID: UserID used to bind in order to search/update user objects
* LDAP_PWD: Password for the related UserID
* LDAP_BASEDN: Base DN where to search for user objects
* LDAP_USER_FILTER: User filter to search for user objects
* LDAP_UPDATE: Update user object with proper Mobile ID SerialNumber of the DN
* LDAP_ATTR_MOBILE: Attribute with the mobile number (default: mobile)
* LDAP_ATTR_LANGUAGE: Attribute with the preferred user language (default: preferredLanguage)
* LDAP_ATTR_SNOFDN: Attribute with the Mobile ID SerialNumber in the DN (default: msNPCallingStationID)
* DEFAULT_LANGUAGE: Default language if not set or invalid [en (default), de, fr, it]
* UNIQUEID_CHECK: Unique Mobile ID (SN of DN) verification [ifset (default), required, ignore]
* ALLOWED_MCC: List of comma separated allowed Mobile Country Codes


## Testing

```
 $ echo "User-Name=samaccountname,User-Password='ADUserPwd'" | radclient -x -t 120 172.17.0.2 auth ThisMustStaySecret
```

## Show logs
```
 $ docker logs freeradius-mobileid
```

