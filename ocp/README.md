oc create sa ansible
oc adm policy add-role-to-user admin -z ansible
oc sa get-token ansible


