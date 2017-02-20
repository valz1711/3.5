1) In Hadoop 1.x there is a main disadvantage related to NameNode (ie) Single point of Failure(if name node fails there will  be some loss of data despite having a secondary node as the updation from name node to secondary name node is done on  hourly basis)
So we come with the solution of HIGH AVAILABILITY OF NODE 
In Hadoop 2.x we have two name node (other than secondary name)namely
1.Active NameNode
2.Standby NameNode
Now  the Data node will send block reports to both Active NameNode and Standby NameNode
So if active NameNode fails or crashes Stand By Name node becomes active NameNode
Then how do we know how Active NameNode fails  .So for that we have Journal Node 
This works on the Basis of polling so if there are 3 journal nodes formula for calculated quorom is
(no of journal node + 2)/2(suppose 3 journal node (3+2)/2 =2)) while the expected QUOROM here is 2.
If active name node fails the journal count quorom will be less than 2 and the journal node automatically changes the 
Stand by name node as active name node and when the failed name node gets up it will now be a standby and the process goes on 
 
2) CHECK POINTING
As we know that NameNode contains Fsimage (which will be have a snapshot or image that contains the MetaData )and the edit logs contain all the transaction after a particular Fsimage  that will be sent to Secondary Name node when the cluster is up(or on an hourly  basis).So if Name Node crashes  and if it gets up again it will have only Last Fsimage and all transactions after it will be lost but fortunately all these information will be in EDIT LOG and again the Secondary name node now becomes name node so it will perform  all the transactions in edit log will again be performed but it takes a long time.So checkpoint comes here we will give a time after which checkpoint is triggered.when a check point is triggered secondary name node will make a RPC call to
NameNode that will fetch the recent FSimage(say f7) and it will get the most recent transaction details say from 7 to 20 from the edit logs ,so now secondary node will merge all edits upto 20 with f7 and create fsimage(say f20.check) and give it to primary name node  and this will be renamed as fsimgae (say f20) and all the Edits in the mean time of this merging  will be stored separately in name Node.So if Name Node goes down only a little time takes as we have a much recent fsimage so there will no delay as in earlier case   

3) HADOOP  FEDERATION 
In Hadoop 1.x  version there is only one name node which has a memory of 64 gB which could maintain  a cluster of 4000  data nodes.With increase in Data generation we are now running towards a scenario where say 10,000 data nodes may be required.SO  MORE MEMORY in NAMENODE is required and the SCALE UP OF A SINGLE NAME NODE ABOVE 64 GB but these results in overhead cost.So in Hadoop 2.x we have a feature called FEDERATION where instead of a single name node set(i.e active name node,passive name node and secondary name node ) there are Multiple name node set(say 3) such that each name node maintains the METADATA of each department separately(say one name node takes care of marketing data alone and other name node takes care of finance  data alone) but all the name nodes can access all DATA NODES but they will only look at related information(ie MARKETING NAME NODE TAKES METADATA ABOUT ONLY MARKEETING  AND FINANCE NAME NODE TAKES METADATA ABOUT ONLY FINANCE) 

4) The four files that need to be configured explicitly while setting up a single node hadoop cluster are:
Core-site.xml
HDFS-site.xml
YARN-site.xml
xml
We can override some explicit properties by configuring them in above files.
In Hadoop, default replication factor is 3 but we can override that property by making replication factor as 1 by explicitly configuring the property in hdfs-site.xml.

Overriding the default parameters optimizes the cluster, improves performance and lets one know about the internal working of Hadoop ecosystem.
