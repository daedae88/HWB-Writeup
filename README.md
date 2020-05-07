07/05/20 - Shibboleth IDP setup for Wales HWB access

This is a write up for anyone in Wales that wants to use Welsh Government HWB facility and they want to use their own Shibboleth server for authentication.

To be frank, I had a nightmare getting this working, with no real help. So I've decided to write up what I had to do, to save someone else the time and frustrations I had.

please see the files, with the writeups, this is what you need to do.

##########
##  N.B
##  IMPORTANT NOTICE
##  ################
##  THIS INFORMATION IS GIVEN FREELY WITHOUT SUPPORT OR ANY ACCEPTANCE OF LIABILITY FOR DAMAGES CAUSED
##  THIS WAS SET UP ON SHIBBOLETH IDP VERSION 3.4.6 ON MICROSOFT WINDOWS
##  IT HASNT BEEN TESTED ON VERSION 4 YET
##  THIS WORKED FOR US. DONT COME CRYING IF IT DOESNT WORK FOR YOU!
##  IF YOU BREAK YOUR OWN SHIBBOLETH SERVER, ITS YOUR FAULT
##  IF YOU COPY THIS PLEASE KEEP THE INFORMATION IN THIS FORMAT AND INCLUDE THIS README.md Files
##########

HWB will give you a document and your relyingparty endpoint, this is a URL formatted like this https://yourcollegelogin.hwb.gov.wales

You will be using this link in the configuration when you set it up.

The Saml2 file you need for your metadata-providers.xml is available for download at http://yourcollegelogin.hwb.gov.wales/Saml2

steps involved:
---------------
When you contact HWB, you will have to send them your PUBLIC shibboleth SSL certificate, this is available in the UKFEDERATION list. if you are getting it from your actual shibboleth server *FOR THE LOVE OF GOD DO NOT SEND OUT YOUR SHIBBOLETH PRIVATE KEY!* when you send this you will also be required to give your shibboleth endpoint. This will be similar to this one:

  example 1:
    https://idp.yourcollegedomain.ac.uk/idp/profile/SAML2/POST/SSO
  example 2:
    https://idp.mycollege.ac.uk/idp/profile/SAML2/POST/SSO

To get this working, you need to make additions to your configuration files. The files you need to modify are:

  1) conf/attribute-filter.xml
  2) conf/attribute-resolver.xml
  3) conf/metadata-providers.xml
  4) conf/relyingparty.xml
  5) and a saved metadata file you can get from HWB itself (from https://yourcollegelogin.hwb.gov.wales/Saml2) saved into metadata/HWB-metadata.xml (as detailed above)

If you take the files here and modify them where necessary, then restart your shibboleth server, (once HWB have activated the configuration their end) you should be good to go.

With regards to the horrible URL for access, I created a hwb.collegedomain.ac.uk host and permanently redirected this with our Apache webserver to point to the horrible URL. It works well! so we give out the host https://hwb.yourcollegedomain.ac.uk to our users

extra diagnostics:
------------------
If you want to monitor what is being released you can modify your conf/logback.xml file to DEBUG then view the IDP logfiles, there you will see the whole SAML2 transaction, along with the attributes being released. You should turn the DEBUG values back to what they were though as you will spam your logs!

  good luck
