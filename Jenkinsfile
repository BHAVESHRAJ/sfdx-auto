#!/bin/bash

echo '=========================== v2.3 ================================='
cp /fullpath/sfdx-project.json ./sfdx-project.json
sfdx force:auth:list

echo '======================= Login to SF =============================='
#Login to SF
sfdx force:auth:jwt:grant --username "${USERNAME}" --jwtkeyfile /fullpath/server.key --clientid "${PASSKEY}" --setdefaultusername --instanceurl https://test.salesforce.com
exit_on_error $? '' 'SF auth process failed'
sfdx force:auth:list

echo '======================= Deploy Source to SF ======================'
#Deploy sources # -l RunAllTestsInOrg | RunLocalTests -c
sfdx force:source:deploy --sourcepath ./force-app/main/default/ -l RunLocalTests
exit_on_error $? 'sfdx force:auth:logout --all -p' 'Source pushing process failed'

echo '======================= Exit All Orgs ============================'
sfdx force:auth:logout --all -p
