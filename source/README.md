[![CircleCI](https://circleci.com/gh/vzakharchenko/mikrotik-hotspot-oauth.svg?style=svg)](https://circleci.com/gh/vzakharchenko/mikrotik-hotspot-oauth)  
[![donate](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://secure.wayforpay.com/button/be27056b0a2b4)  

# Keycloak Radius Hotspot
Setup social and other Oauth/Saml integration with [Keycloak Radius embedded server](https://github.com/vzakharchenko/keycloak-radius-plugin/releases)  
# How Keycloak Radius Hotspot works  
1. Authorization through Keycloak occurs by [OpenID Connect](https://www.keycloak.org/docs/latest/securing_apps/#openid-connect-2).  
2. User selects on the login page the identity provider through which he wants to log in  
3. The result of a successful authorization is a JWT that contains a temporary session key.  
4. With this key, the User is authorized through Radius Server.  
5. Radius Server checks if this key is in the user session. And whether it was used.  
6. Radius Server successfully authorizing the user  

# connection Schema`s

## - Cloud connection (Better to Use Radsec)
![KeycloakRadius (1)](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/KeycloakRadius.png)


## - Proxy connection
![KeycloakRadius](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/KeycloakRadius2.png)

# Setup, build and configure  HotSpot page for Social Login

1. Create Realm ![hotspotRealm](/docs/hotspotRealm.png)
2. create Radius Client ![RadiusClientHotSpot](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/RadiusClientHotSpot.png)  
3. create OpenId client ![hotspotClient](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/hotspotClient.png)  
4. Setting your Hotspot DNS in "Valid Redirect URIs" and "Web Origins" ![HotspotClientConfiguration](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/HotspotClientConfiguration.png)  
5. add "Radius Session Password" Mapper ![HotSpotMapper](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/HotSpotMapper.png) ![HotSpotMapper2](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/HotSpotMapper2_1.png)  
6. Download keycloak.json ![downloadKeycloakJson](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/downloadKeycloakJson.png)  

##  Setup Mikrotik
1. Upload all files from [hotspot/mikrotik](mikrotik) to flash/hotspot on device ([authorization.js](mikrotik/authorization.js) and [login.html](mikrotik/login.html))  
-  Using web UI  
-  Using scp  
- Using ftp  
- Using winbox  
2. Download keycloak.json ![downloadKeycloakJson](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/downloadKeycloakJson.png)  
3. upload keycloak.json into flash/hotspot on device  
4. update Walled Garden. Add your keycloak host ![addWalledGarden](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/addWalledGarden.png) ![KeycloakHostName](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/KeycloakHostName.png)  

## Facebook Login example
1.  [Install Keycloak with embedded Radius Server](https://github.com/vzakharchenko/keycloak-radius-plugin#release-setup)
2.  install [ngrok](https://ngrok.com/). Register ngrok  <pre><code>./ngrok authtoken \<YOUR TOKEN\></pre></code>
3. start ngrok <pre><code>./ngrok http 8090</pre></code>![Ngrok](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/Ngrok.png)
4. open keycloak goto realm and add Facebook Identity Provider ![SelectFacbook](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/SelectFacbook.png)
5. Copy Redirect URI ![Copy Redirect URI](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/Copy%20Redirect%20URI.png)
6. goto [https://developers.facebook.com/](https://developers.facebook.com/) and create a new application ![CreateApp1](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/CreateApp1.png)![CreateApp2](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/CreateApp2.png)![FacebookLogin3](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/FacebookLogin3.png)![Facebook4](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/Facebook4.png)
7. Insert Redirect URI from [Step 7](#L43) ![Facebook5](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/Facebook5.png)
8. Get App Id and Secret from application (Settings->basic) ![Facebook6](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/Facebook6.png)
9. back to Keycloak and set this App Id and Secret ![Facebook7](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/Facebook7.png)
10. add facebook hosts to Walled Garden ![FacebookWalledGarden](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/FacebookWalledGarden.png)  
```
/ip hotspot walled-garden  
add comment=facebook dst-host=facebook.*  
add comment=facebook dst-host=*.facebook.*  
add comment=facebook dst-host=*.fbcdn.*  
add comment=facebook dst-host=*akamai*  
add comment=facebook dst-host=*atdmt*
add comment=facebook dst-host=*fbsbx*
add comment=common dst-host=www.google-analytics.com
```

12. open hotspot page ![FacebookLoginHotspot](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/FacebookLoginHotspot.png) ![FacebookLogin2](https://github.com/vzakharchenko/mikrotik-hotspot-oauth/raw/master/docs/FacebookLogin2.png)  



## build UI

### build UI Requirements:
node and npm must be installed  
macbook instalation [brew](https://brew.sh/) : *brew install node*  
[Install node on ubuntu ](https://linuxize.com/post/how-to-install-node-js-on-ubuntu-18.04/)  

### Building steps
1. cd [./source](source)  
2. npm i  
3. npm run build  
result in [./mikrotik](mikrotik)  

# If you find these useful, please [Donate](https://secure.wayforpay.com/button/be27056b0a2b4)!  
