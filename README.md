This project includes:

1. ANT project, named myec-stats, for simple descriptive statistics. 
2. Three EJB Session beans

	2.1 A singleton session bean, named StatsEJBSingleton, with local and remote interfaces MyStatsSingletonLocal, MyStatsSingletonRemote. 
		Both interfaces contains the following methods.
			addData(double): adding a data element
			getCount(): returning the number of elements
			stats() : computing the descriptive statistics
			saveModel(): saving the serializable object, StatsSummary (just contains the attributes of the simple statistics), to file C:/tmp/enterprise/model/stats.bin
		The StatsEJBSingleton contains array list to hold data of double type and implementations of the interface methods.
	
	2.2 A stateless session bean, named StatsEJBStateless, local and remote interfaces StatsEJBStatelessLocal, StatsEJBStatelessRemote. 
		Both local and remote interfaces contain methods.
			getCount(): returning the data count
			getMin():   getting the minimum element
			getMax():   getting maximum element
			getMean():  getting the mean
			getSTD():   getting the standard deviation
			toString(): returning a String of the simple stats summary
		The StatsEJBStateless contains a load method loadModel(), which load the C:/tmp/enterprise/model/stats.bin and return StatsSummary object. StatsEJBStateless also contains the implementation of interface methods. Each of them will call load() function and get the required properties from the returned StatsSummary object.
	
	2.3 A stateful session bean, named StatsEJBStateful, local and remote interfaces StatsEJBStatefulLocal, StatsEJBStatefulRemote. 
		The StatsEJBStateful injects StatsEJBSingleton 	and	  StatsEJBStateless. Both interfaces contains methods:
			insertData(double): to the StatsEJBSingleton object.
			createModel() : save the stats summary model StatsEJBSingleton. 
			getStats() : get the stats summary string by the StatsEJBStateless object. 
3. EJB client, named stats-ejb-client, which tests the remote interface methods of the three EJBs
4. A Web component, named stats-ejb-web, which contains three Servlet programs:
   
   4.1 StatsEJBSingletonServlet, which inject StatsEJBSingleton, and get data from query and call addData method to add the query data into the StatsEJBSingleton array list, 
		and then called saveModel. The Servelet returns an HTML page saying ?? added, model saved.
	
	4.2 StatsEJBStatelessServlet, which inject StatsEJBStateless, and get query message, e.g. count, min, max, mean, or std, summary, and return call the corresponding method 
		of StatsEJBStatelessLocal, and return result in HTML.
	
	4.3 StatsEJBStatefuleServlet, which inject StatsEJBSingleton and StatsEJBStateless, and gets query insert value from html form query, call StatsEJBSingletonLocal methods 
		addData to insert the data to array list, call saveModel method to save the model, and finally add call getStats() method of StatsEJBStateless, and return results in HTML.
	
	4.4 An index.html which has three forms for queries for actions of above servelets programs.
