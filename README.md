**This repository is about migration games framework** 

File 'Европейская боль' contains the real-data content for modelling the voting games within 34 EU countries and 1.34mln. To model the migration flow use coding block 3, to get the basis of the model and search for dependencies in the voting games.
File 'ПограничныеСлучаиБоли' is used for the pacticular case modelling with strongly shaped graph reproduction or even any modelling data you want. To run it properly you need:
1. Form your own graph structure with 7 parameters used in the model:

   [Population 2022 (thousands), GDP at 2022 (thousands), medianNetSalary (eu/hour in prices of 2022), avgWageShift per 1k migrants, wage per capita(wage*Labour force/GDP), wageElasticity (dlog(|Shift|)/dlog(|LF|)), integrationCosts (thousands euro per migrant)]
 
2. Adjust the edges structure of your own graph. Add at least 1 vertex connected to 's' (you'd get 0 iteration else) and at least 1 connected to 'q' (you'd get infinite steps otherwise).  listOfEdges =[['s','smth1'], ..., ['smthN','q']]
 
 Change the parameters of agents on vertices:

       verticesDict = {'s': [w_In, 0, 0, 0, 0, 0, 0],
                     '1': [P1, P2, P3, P4, P5,  P6, P7],
                     ///
                     'q':[1000000, 0, 0, 0, 0, 0, 0]
     }
**Do not touch 's' and 'q'**

3. Adjust the weights on the edges. Omega is the weight. a - your number

       def omegaCount(G, edgeNumber):
        omega = min(verticesDict.get(listOfEdges[edgeNumber][0])[0]*a, verticesDict.get(listOfEdges[edgeNumber][1])[0]*a)
        return omega

4. To reveal and show the graph structure and visualize uncomment it...

       #elarge = [(u, v) for (u, v, d) in G.edges(data=True) if d["weight"] > k]
       #esmall = [(u, v) for (u, v, d) in G.edges(data=True) if d["weight"] <= k]
     
       #pos = {'s': (PosVectical, PosHorizontal), '1':(PosVectical, PosHorizontal),... , 'q':(PosVectical, PosHorizontal)}
     
       #options = {
           #"font_size": 18,
           #"node_size": 6000,
           #"node_color": "white",
           #"edgecolors": "black",
           #"linewidths": 5,
           #"width": 10,
       #}
       #plt.figure(figsize =(16,16))
       #nx.draw_networkx(G, pos, **options)
 
   ... and put into the modelling function starting at the row '#Graph representation here'

 6. To work with the outputs you want to use 'dfPL10mln' pandas dataframe. To use the basic output don't touch the DF block

         for i in range(len(OverallProfitForIterationList)):
           dfPL10mln.loc[len(dfPL10mln)] = [i, OverallProfitForIterationList[i], FlattenedInterQlistTotal[i], OverallProfitForIterationList[i]-FlattenedInterQlistTotal[i], MFbyIterReal[0][i], MFbyIterReal[1][i], MFbyIterReal[2][i], MFbyIterReal[3][i], MFbyIterReal[4][i]]*

 If you need any other data, follow the logic of model outputs listed here:
 
 >(iterations, MigrantsFlowByPlayer, TP, CoalitionOfAgreement, CoalitionOfAgreementProfit, ProfitsOfAllAgents, OverallProfitForIterationList, MQ, TQ = FullCFModel(unit, listOfEdges, verticesDict, alpha, crimeRatio, remittancesRatio, reproductionRatio, socialEnthropyRatio))
 
   ~~~
    iterations - the iteration where quote of votes for stopping has been reached; 
    MigrantsFlowByPlayer - matrix with agents as columns and MF as rows; 
    TP - total profit as a sum of profit list of all agents; 
    CoalitionOfAgreement - list of agents voted to end the game at 'iterations'; 
    CoalitionOfAgreementProfit - sum of the CoalitionOfAgreement profits; 
    ProfitsOfAllAgents - list of profits of all agents at 'iterations'; 
    OverallProfitForIterationList - matrix of all agents' profits with agents as columns and iterations as rows; 
    MQ - string of migrants left the MF at each iteration; 
    TQ - sum of list of cost of each agents at the each game iteration
