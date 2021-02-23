# JBoss EAP Operator Demo

This demo demonstrates how to use the new JBoss EAP Operator, based on the 
community (Wildfly Operator)[https://github.com/wildfly/wildfly-operator].

## Steps
1. Create the template in your project. The template contains only configurations
   related to BuildConfigs and ImageStreams for the artifacts build and the
   final image build.
   ```
   $ oc apply -f eap73-build-template.yaml
   ```
2. Process the template using default values and pipe to the oc CLI to create 
   objects and automatically start the build. The example will build and deploy
   the **kitchensink** application in a standalone JBoss EAP 7.3 instance.
   ```
   $ oc process eap73-build-s2i | oc apply -f -
   ```
3. Once the build is finished, create a `WildflyServer` object that manages
   the deployment of the resource, together with persistance, services and 
   routes.
   ```
   $ oc apply -f wildflyserver-example.yaml
   ```

## TODO
- Refactor the whole example using Helm.
