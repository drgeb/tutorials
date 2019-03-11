## Relevant articles:
- [A Quick Guide to Using Keycloak with Spring Boot](http://www.baeldung.com/spring-boot-keycloak)


export KEYCLOAK_HOME=/opt/jboss/keycloak
export PATH=$PATH:$KEYCLOAK_HOME/bin
export REALM=SpringBootKeycloak
export CLIENT_ID=login-app
export ROLENAME=user
export USERNAME=test_username
export firstName=test_firstname
export lastName=test_lastname
export email=testemail@drgeb.com
kcadm.sh config credentials --server http://localhost:8080/auth --realm master --user admin --password admin123
kcadm.sh create realms -s realm=${REALM} -s enabled=true
kcadm.sh create clients -r ${REALM} -s clientId=${CLIENT_ID} -s "redirectUris=[\"http://localhost:8081/*\"]" -i > ${CLIENT_ID}_clientId.txt

more login-app_clientId.txt
3305deec-bbaf-45b8-aeb1-08f319aa208c
kcadm.sh create roles -r ${REALM} -s name=${ROLENAME} -s 'description=Regular user with limited set of permissions'
kcadm.sh create users -r ${REALM} -s username=${USERNAME} -s firstname=${FIRSTNAME} -s lastName=${LASTNAME} -s email=${EMAIL} enabled=true
Invalid option: enabled=true
Try 'kcadm.sh help create' for more information
kcadm.sh add-roles  -r ${REALM} --username ${USERNAME} --rolename ${ROLENAME}
Unknown command: -r
kcadm.sh create users -r ${REALM} -s username=${USERNAME} -s firstname=${FIRSTNAME} -s lastName=${LASTNAME} -s email=${EMAIL} -s enabled=true
HTTP error - 400 Bad Request
kcadm.sh config credentials --server http://localhost:8080/auth --realm demo --user admin --client admin123
Logging into http://localhost:8080/auth as user admin of realm demo
Enter password: ********
404 Not Found
kcadm.sh config credentials --server http://localhost:8080/auth --realm master --user admin --password admin
Logging into http://localhost:8080/auth as user admin of realm master
Invalid user credentials [invalid_grant]
kcadm.sh config credentials --server http://localhost:8080/auth --realm master --user admin --password admin123
Logging into http://localhost:8080/auth as user admin of realm master
kcadm.sh create users -r ${REALM} -s username=${USERNAME} -s firstname=${FIRSTNAME} -s lastName=${LASTNAME} -s email=${EMAIL} -s enabled=true
HTTP error - 400 Bad Request
kcadm.sh create users -r ${REALM} -s username=${USERNAME} -s firstname=${FIRSTNAME} -s lastName=${LASTNAME} -s email=${EMAIL}
HTTP error - 400 Bad Request
kcadm.sh create users -r ${REALM} -s username=${USERNAME}
Created new user with id '4148e293-4ec6-47ac-8dc9-f7d3f34a5d80'
kcadm.sh create users -r ${REALM} -s username=${USERNAME} -s firstname=${FIRSTNAME} -s lastName=${LASTNAME} -s email=${EMAIL}
HTTP error - 400 Bad Request
kcadm.sh create users -r ${REALM} -s username=${USERNAME} -s firstname=${FIRSTNAME} -s lastName=${LASTNAME}
HTTP error - 400 Bad Request
kcadm.sh create users -r ${REALM} -s username=${USERNAME} -s firstname=${FIRSTNAME}
HTTP error - 400 Bad Request
kcadm.sh create users -r ${REALM} -s username=${USERNAME} -s enabled=true
User exists with same username
kcadm.sh add-roles  -r ${REALM} --username ${USERNAME} --rolename ${ROLENAME}
Unknown command: -r
kcadm.sh add-roles --username ${USERNAME} --rolename ${ROLENAME} -r ${REALM}
Unknown command: --username
kcadm.sh add-roles  -r ${REALM} --username ${USERNAME} --rolename ${ROLENAME}
Unknown command: -r
kcadm.sh add-roles --username ${USERNAME} --rolename ${ROLENAME} -r ${REALM}
Unknown command: --username
kcadm.sh add-roles
Usage: kcadm.sh add-roles (--uusername USERNAME | --uid ID) [--cclientid CLIENT_ID | --cid ID] (--rolename NAME | --roleid ID)+ [ARGUMENTS]
       kcadm.sh add-roles (--gname NAME | --gpath PATH | --gid ID) [--cclientid CLIENT_ID | --cid ID] (--rolename NAME | --roleid ID)+ [ARGUMENTS]
       kcadm.sh add-roles (--rname ROLE_NAME | --rid ROLE_ID) [--cclientid CLIENT_ID | --cid ID] (--rolename NAME | --roleid ID)+ [ARGUMENTS]

Command to add realm or client roles to a user, a group or a composite role.

Use `kcadm.sh config credentials` to establish an authenticated session, or use CREDENTIALS OPTIONS
to perform one time authentication.

If client is specified using --cclientid or --cid then roles to add are client roles, otherwise they are realm roles.
Either a user, or a group needs to be specified. If user is specified using --uusername or --uid then roles are added
to a specific user. If group is specified using --gname, --gpath or --gid then roles are added to a specific group.
If composite role is specified using --rname or --rid then roles are added to a specific composite role.
One or more roles have to be specified using --rolename or --roleid so that they are added to a group, a user or a composite role.

Arguments:

  Global options:
    -x                    Print full stack trace when exiting with error
    --config              Path to the config file (~/.keycloak/kcadm.config by default)
    --no-config           Don't use config file - no authentication info is loaded or saved
    --token               Token to use to invoke on Keycloak.  Other credential may be ignored if this flag is set.
    --truststore PATH     Path to a truststore containing trusted certificates
    --trustpass PASSWORD  Truststore password (prompted for if not specified and --truststore is used)
    CREDENTIALS OPTIONS   Same set of options as accepted by 'kcadm.sh config credentials' in order to establish
                          an authenticated sessions. In combination with --no-config option this allows transient
                          (on-the-fly) authentication to be performed which leaves no tokens in config file.

  Command specific options:
    --uusername           User's 'username'. If more than one user exists with the same username
                          you'll have to use --uid to specify the target user
    --uid                 User's 'id' attribute
    --gname               Group's 'name'. If more than one group exists with the same name you'll have
                          to use --gid, or --gpath to specify the target group
    --gpath               Group's 'path' attribute
    --gid                 Group's 'id' attribute
    --rname               Composite role's 'name' attribute
    --rid                 Composite role's 'id' attribute
    --cclientid           Client's 'clientId' attribute
    --cid                 Client's 'id' attribute
    --rolename            Role's 'name' attribute
    --roleid              Role's 'id' attribute
    -a, --admin-root URL      URL of Admin REST endpoint root if not default - e.g. http://localhost:8080/auth/admin
    -r, --target-realm REALM  Target realm to issue requests against if not the one authenticated against

Examples:

Add 'offline_access' realm role to a user:
  $ kcadm.sh add-roles -r demorealm --uusername testuser --rolename offline_access

Add 'realm-management' client roles 'view-users', 'view-clients' and 'view-realm' to a user:
  $ kcadm.sh add-roles -r demorealm --uusername testuser --cclientid realm-management --rolename view-users --rolename view-clients --rolename view-realm

Add 'uma_authorization' realm role to a group:
  $ kcadm.sh add-roles -r demorealm --gname PowerUsers --rolename uma_authorization

Add 'realm-management' client roles 'realm-admin' to a group:
  $ kcadm.sh add-roles -r demorealm --gname PowerUsers --cclientid realm-management --rolename realm-admin

