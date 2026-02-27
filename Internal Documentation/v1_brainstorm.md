# Step 1
Collect a data set of students of the form (first name, last name, class year) and store it in a CSV file.

# Step 2
For each student in the data set, apply LLM_policy_1 to determine their imediate family members

* We are essentially createing a file on the family of each student in the data set (students in the same family will have the same family file)

* An secondary iteration of this step will apply LLM_policy_1 on each family member in order to grow the family tree and add that data to the family_file

## family_file
This data structue will contain the 

## LLM_policy_1 (p1)
* This policy should be able to function with or without the personal_data collected in through LLM_policy_2

* if given the addition of personal_data the data collected in LLM_policy_2, p1 should be able to provide a better output (more family members). Although this is only a theory, we will study the results of p1 with and without the personal_data to see if this is the case. this study will be the one of the fudmental basis of the graph_df

* This needs to be highly stable no matter what stage in the program it is called is. It needs to not corrupt the family_file or personal_data, only add information



# Step 3
We need to develop a llm policy that can is able to input use family_file (and existing personal_data) to output data in the structure of personal_data using LLM_policy_2

This could work in a number of ways:
* We apply LLM_policy_2 to each member of the family in family_file. This is a highly modular and token efficient system

* We apply LLM_policy_2 to the entirety of family file. This uses the connections that the personal_data of each family member would be able to form to provide better data. Theory: If we add more information to the personal_data then we will be able to identify more nodes of the graph_df.



## LLM_policy_2
The purpose of LLM_policy_2 is to generate or expand upon the personal_data for an individual or family file

* 


## personal_data
This is a data structure contained within family_file for each member of the family

* If members in family_file share information (such as live in same household and share assets) then these data structures should be linked. Linking not only means they are updated together but also means that when attempting to apply LLM_policy_2 to a family member you should take into account the personal_data from any member that is linked to the member in question (or if we are using data from all family members then you should consider the data from linked members differently)

* members will also be linked by their relationships. This link is different b/c the data is not going to be identical so the data structure will need to take into account inverse relationships.

### High level catagories of columns (not the actual columns)

#### Assets



#### Early Life
* How did they obtain their status

#### Relationships
* Siblings
* parents
* freinds

#### Education

#### Career
* industry
* Salary
* title
* Company
* history
* internships

#### Affiliations
* Orginizations
* Non-profits
* trusts
* start-ups
* projects

#### Internt Accounts
* Social Media: Linkedin, Instagram, Facebook
* Emails: Google, outlook, icloud, institution, work
* Github

#### MIT Specific
* major
* classes
* research
* 





# Future Works

## Graph Theory Repersentation of a digital footprint (graph_df)
tbd - will not be implented in the MVP
