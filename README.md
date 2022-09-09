* connection debug
This is a project to highlight the connection issue that we have been coping with 
in the install and deploymoent of xdmod

* deploy the project

        oc apply -k k8s/kube-base

* get the xdmod-pod name

        oc get pods 

* replace xdmod-pod with the xdmod-pod name

        oc rsh xdmod-pod 

* then run

        % xdmod-init


Something that worked around that may be related, line ~293 I had to put a sleep as 
it seem that a write to the attached volumn was failing.

After doing this, I relize there is more code that could be eliminated
