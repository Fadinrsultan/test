## Class production plan

**Production plane and execution for one machine.**

**Implements :** planning step including reorder point,economic quantity and order scheduling /actual scheduling/production step.

### **Function *Plan***


Adds production jobs,"produce" routine eliminates them again.It starts with adding new orders (generating new demand) then takes demand /inventory pipeline and fills the job pipline.

**Procedures**

*1.Iterate over the demands/inventories queue.*

 1.1. Separate demand and inventory and product in different varieties.
 
 1.2. Generate demand and forecast for 1 day:
   

       If the current inventory level for a product is <= than the reorder point and replenishment due of a product is <= than 0.
       If replenishment open of a product is greater than  o THEN  calculate the EOQ dynamic with the actual demand (only first element ) and the next forcasts.
       for the calculation cost per piece (for eoq), cost per batch and inventory interest (and eoq mode)

       If eoq > 0 
          THEN 1. add a new element to the Joblist containg anount =eoq, current date ,inventory (current ) and relevant product.
               2.decrease the replenishment open by the eoq.

### **Function *Schedule***

Schedule the Joblist using different methods:Value,SPT,LPT, Slip.

    If scheduling regime= Value:
       the Joblist is produced not in the ascending order ,but the Joblist is then sorted by value = multiplying amount by cost per piece for eoq.
    If scheduling regime = SPT:
       ordering according to Time.
       the joblist is sorted by value= multiplying  the amount by production time per piece 
    If scheduling regime = LPT
       Ordering similar to SPT but in the opposite direction.
    If scheduling regime = Slip
       the joblist is sorted by value =subtracting  the date by (amount multiplied by production time per piece)
       sort the joblist by sorts and ascending value ,eliminate sorts.

### **Function *Produce***

 It perform the following points:
 
1.Executes the first Joblist from the list and eliminated the entry.It return the duration of the production job , amount of parts produced and inventory.

    If length of the Joblist is 0
     THEN return 0,0,0 for duration , amount and inventory.
     
2.Place the amount, inventory and product in separate variates and production amount must be greater than 0 .

3.Calculate the duration to produce the given amount by sampling from Log-normal distribution with mean =amount*production time per piece and sigma=varcoef/ √(amount)

4.Increase the production time (total) by the sampled duration.

5.Calculate the setup time by sampling from Log-normal distribution with the mean setup time per batch and sigma, VarCoef (of a machine),But only in the Case if last     product is different than the current one (no setup time nedded if the product is the same).

6.Increase the duration of production by the setup time .

7.Add the setup time to the total setup time .

8.Update the last product variable.

9. Delete the first element from the joblist.

10.Return the calculated duration of production Job, the amount produced and inventory.


