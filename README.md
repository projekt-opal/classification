# OPAL Classification

This component predicts objects of the dcat:theme predicate of dcat:Dataset subjects, based on the dc:description property. The implementation was done using WEKA https://github.com/Waikato/weka-3.8.
You can run the application with the default values with <br />`mvn clean install` and `mvn exec:java -Dexec.mainClass="tools.Main" -Dexec.args="-c j48 -ngrams 1" -Dexec.cleanupDaemonThreads=false`<br /> 
The result of the evaluation of the cross-validation of the training data and the evaluation of the test data is printed to console.

The following arguments can be provided: <br />
**-c {naive, j48}**, default it j48<br />
**-ngrams {1,...,n}**, default is 1 <br />
**-query**, sparql query default is 
`SELECT  *
WHERE
{ ?s  a                     <http://www.w3.org/ns/dcat#Dataset> ;
               <http://www.w3.org/ns/dcat#theme>  ?o ;
         <http://purl.org/dc/terms/description>  ?d
     FILTER ( lang(?d) = "en" )
} LIMIT   300 `<br />
**-endpoint**, sparql endpoint, default is: https://www.europeandataportal.eu/sparql

### Pre-processing
The following steps were taken:<br />
1. Removed punctuation <br />
2. Converted all text to lower case <br />
3. Tokenization and Lemmatization <br />
4. Removed the standard english stop words if either the lemma or the original word coincides <br />


### Word vectorization
The standard TF-IFD word vectors were computed.


## Results
In the interest of time and since the approach is slow, the classifier was trained with 160 instances. That number might be too small to be representative.<br />
The following accuracy was obtained for the cross-validation method with 4 folds:

| Classifier    | 1-gram        | 2-gram       |3-gram              | 4-gram       | 
| ------------- |:-------------:|:------------:|:------------------:|:------------:|
| J48           |75,625%         |59,375%           |59,375%               |       59,375%    |
| NaiveBayes    | 47,5%         |   31,875%        | 36,875%              | 35%        |

The following accuracy was obtained for the evaluation of the test data.

| Classifier    | 1-gram        | 2-gram       |3-gram              | 4-gram       | 
| ------------- |:-------------:|:------------:|:------------------:|:------------:|
| J48           | 62,07%         |50%           |59,09%               |       55,32%    |
| NaiveBayes    | 28,09%         |  29,35%        | 27,59%              | 28,05%        |


## Note

This component was developed by Ana Alexandra Morim da Silva during the [OPAL hackathon](https://github.com/projekt-opal/hackathon).
The component was on of the [winners of the hackathon](http://projekt-opal.de/en/opal-open-data-hackathon-2/). 


## Credits

[Data Science Group (DICE)](https://dice-research.org/) at [Paderborn University](https://www.uni-paderborn.de/)

This work has been supported by the German Federal Ministry of Transport and Digital Infrastructure (BMVI) in the project [Open Data Portal Germany (OPAL)](http://projekt-opal.de/) (funding code 19F2028A).
