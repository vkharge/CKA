# Download Presentation Deck
  - Take me to [Presentation Deck](https://kodekloud.com/courses/certified-kubernetes-administrator-with-practice-tests/lectures/16092815)


etcdctl snapshot save -h

etcdctl snapshot restore -h


Since our ETCD database is TLS-Enabled, the following options are mandatory:


	--cacert    verify certificates of TLS-enabled secure servers using this CA bundle
	
	
	--cert    identify secure client using this TLS certificate file
	
	
	--endpoints=[127.0.0.1:2379]    This is the default as ETCD is running on master node and exposed on localhost 2379.
	
	
	--key     identify secure client using this TLS key file


  
References

https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster

https://github.com/etcd-io/etcd/blob/master/Documentation/op-guide/recovery.md


https://www.youtube.com/watch?v=qRPNuT080Hk
