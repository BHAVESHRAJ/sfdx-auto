#!/bin/bash
#Exit from build:
#params: 1 - status, 2 - callback command to call before exit, 3 - comment text for  failed GitHub commit
exit_on_error() {
    if [ $1 -ne 0 ]
    then
        echo '=== ### Exit On Error ### ==='
        if [ "$2" != '' ]
        then
            $2
        fi

        echo 'Failure' "$3"
        exit 1
    fi
}

sfdx --version
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
