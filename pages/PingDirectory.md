- Releases
- v1.16.0.0
  id:: 639b9c34-d2b6-4538-8119-5eefecb74d4c
	- PingDirectory version - 8.3.0.0
		- feature A
		- feature B
	-
- v1.17.0.0
  id:: 639b9cbe-9514-4141-a8bf-4f2f3827969a
	- PingDirectory version - 8.3.0.1
		- feature A
		- feature B
-
- Ping's LDAP datasource product
	- Highly available
	- Optimized for reads in normal deployment setup.
- Inside PingOne Advanced Services PingDirectory is a required application despite the potential that the customer does not want to use it as their primary datastore.
- PingFederate deployments in PingOne Advanced Services makes use of the in cluster PingDirectory to store user tokens and clients for federated sign-ins.
- PingDirectory is deployed as a statefulset in customer environments due to the stateful nature of the data it contains.
- All regions of a multi-region customer will have PingDirectory pods deployed.