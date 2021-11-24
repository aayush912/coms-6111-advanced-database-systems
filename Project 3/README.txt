----- a. Team: -----

	a. Aayush Kumar Verma	-  av2955
	b. Mehul Goel		-  mg4260



----- b. Files submitted: -----

	a. tar -> main.py, INTEGRATED-DATASET.csv
	b. INTEGRATED-DATASET.csv
	c. example-run.txt
	d. README



----- c. How to run the program: -----

	In the Google Cloud VM, run the following commands:
	
		python3 main.py INTEGRATED-DATASET.csv 0.01 0.42


----- d. Detailed description about dataset used: -----

	a. We have used the following dataset:

		HIV/AIDS Diagnoses by Neighborhood, Sex, and Race/Ethnicity (https://data.cityofnewyork.us/Health/HIV-AIDS-Diagnoses-by-Neighborhood-Sex-and-Race-Et/ykvb-493p)

	b.
		Removed 'year' column because we wanted to get more meaningful results with respect to the neighbourhoods.

		To have the 'high-confidence rules' alone be sufficient to infer their meaning from, we prefixed each value of every column with the column name 

		We removed 'HIV DIAGNOSES PER 100,000 POPULATION' and 'AIDS DIAGNOSES PER 100,000 POPULATION' column as their didn't seem consistent with respect to the whole data set and the column header

		We removed 'PROPORTION OF CONCURRENT HIV/AIDS DIAGNOSES AMONG ALL HIV DIAGNOSES' and 'TOTAL NUMBER OF CONCURRENT HIV/AIDS DIAGNOSES' column as it had data derived from other columns

		We changed the column names as follows:
			Neighborhood (U.H.F) - neighbourhood
			RACE/ETHNICITY - ethnicity
			TOTAL NUMBER OF HIV DIAGNOSES - total_hiv_cases
			TOTAL NUMBER OF AIDS DIAGNOSES - total_aids_cases
			
		We have put numeric data of TOTAL NUMBER OF HIV DIAGNOSES and TOTAL NUMBER OF AIDS DIAGNOSES into categories: {low, medium, high} with the limits as if <=5 category='Low', if (num<=10 and num>5) category=Medium' if (num>10) category='High'. This was done so that the A-priori algorithm has a better chance to find any rules at all taking along the numeric value columns, as the exact value of the numeric data, apart from the '0' value, gets repeated rarely.

		We removed the rows with ethnicity=All and neighbourhood=All as they represented the combined data of multiple rows which was inconsistent with the indivudual rows of the dataset

	c. 
		We found this data set interesting because we wanted to detect any pattern in the spread of HIV and AIDS cases in New York, mainly neighbourhood and ethnicity wise



----- 5. Internal working of the project: -----

	Step-1:
		
		a. Create a list containing support of each item present in the dataset called C1 (candidate set).
		b. Compare candidate set item’s support with minimum support and if it is less than the minimum support then remove that item. This gives us itemset L1.


	Step-2:
		
		a. Generate candidate set C2 using L1. Condition of joining Ck and Lk-1 is that they should have (K-2) elements in common. Say, for L3, first 2 elements (items) should match.
		b. Check if all subsets of an itemset are frequent or not and if they are not frequent then remove that itemset and now find the support of these itemsets by searching in the dataset.
		c. Compare candidate (C2) supports with minimum support (if support < minimum support, remove that item) to get itemset L2.


	Step-3:

		a. Repeat above steps until no more frequent itemsets are found.
		b. Open a file "example-run.txt" and write the frequent itemsets and the association rules to the file.



----- 6. Command line specification of a compelling sample run: -----

		a. Run the following command in the VM console:
	
			python3 main.py INTEGRATED-DATASET.csv 0.01 0.7
			
		b. Our rules are for the years 2010-2013 as the data was of those years:


			['sex=Female', => 'total_hiv_cases=Low',] (Conf: 89.2027%, Supp:50.0208%)
			Number of HIV cases in females as compared to that in males is low 


			['total_aids_cases=High', => 'total_hiv_cases=High',] (Conf: 95.5466%, Supp:10.2617%)
			['total_aids_cases=Low', => 'total_hiv_cases=Low',] (Conf: 94.45%, Supp:83.8388%)
			HIV and AIDs are correlated


			From the rules containing 'neighbourhood' we infer that most of the neighbourhoods had lesser than 5 cases per year from 2010-2013 because our range for 'Low' was 0-5



			['ethnicity=Native American', => 'total_hiv_cases=Low',] (Conf: 100.0%, Supp:14.2916%)
			['ethnicity=Native American', => 'total_aids_cases=Low',] (Conf: 100.0%, Supp:14.2916%)

			['ethnicity=White', => 'total_hiv_cases=Low',] (Conf: 75.0%, Supp:14.2916%)
			['ethnicity=White', => 'total_aids_cases=Low',] (Conf: 82.2674%, Supp:14.2916%)

			['ethnicity=Asian/Pacific Islander', 'total_hiv_cases=Low' => 'total_aids_cases=Low',] (Conf: 100.0%, Supp:13.4607%)
			['ethnicity=Asian/Pacific Islander', => 'total_aids_cases=Low',] (Conf: 98.2558%, Supp:14.2916%)

			['ethnicity=Multiracial', => 'total_hiv_cases=Low',] (Conf: 99.7093%, Supp:14.2916%)
			['ethnicity=Multiracial', => 'total_aids_cases=Low',] (Conf: 100.0%, Supp:14.2916%)

			The ethnicities: Native American, White, Asian/Pacific Islander and Multiracial have low HIV and AIDS cases. 

			The fact that there were no high-confidence rules found for Hispanic and Black ethnicities shows that their HIV and AIDS cases are spread across low, medium, high ranges when considering all neighbourhoods



			['ethnicity=Hispanic', 'sex=Female' => 'total_hiv_cases=Low',] (Conf: 70.9302%, Supp:7.1458%)
			['ethnicity=Hispanic', 'sex=Female' => 'total_aids_cases=Low',] (Conf: 76.1628%, Supp:7.1458%)

			Number of HIV and AIDS cases in Hispanic females is low


