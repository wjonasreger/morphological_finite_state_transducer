# Morphological Transduction with Finite State Transducers

In this notebook, a simple finite state transducer (FST) is implemented, which transduces the infinitive form of Spanish verbs to the preterite (past tense) form in the 3rd person singular (verb conjugation). A [set of Spanish verbs](https://github.com/wjonasreger/data/blob/main/spanish_verbs.csv) are used to evaluate the FST's accuracy.

---

I implemented a rudimentary Finite State Transducer that can handle tranduction tasks over single character strings. In particular, it transduces Spanish verbs to apply various conjugation rules as shown below.

1. Verbs ending in <TT>-ar</TT> have <TT>-ó</TT> endings.
2. Verbs ending in <TT>-er</TT> and <TT>-ir</TT> have <TT>-ió</TT> endings.
3. Verbs ending in <TT>-er</TT> or <TT>-ir</TT> with stems that end with an <TT>-ñ</TT> have <TT>-ó</TT> endings.
4. Verbs ending in <TT>-er</TT> or <TT>-ir</TT> with stems that end with a vowel have <TT>-yó</TT> endings. This does not apply to regular verbs ending in <TT>-guir</TT> or <TT>-quir</TT>.
5. Verbs ending in <TT>-ir</TT> with stems that end with an <TT>e</TT> followed by any number of consonants will raise the <TT>e</TT> to an <TT>i</TT> and have <TT>-ió</TT> endings. This does apply to verbs with an <TT>-eguir</TT> ending.

The conjugation rules are shown more specifically in the table below. Note that <TT>C</TT>=Consonant, <TT>S</TT>=String, and <TT>V</TT>=Vowel.

| Rule ID  	| Rule Name             | Ending  	| Infinitive           	| Conjugation          	| Example                   	|
|-----	    |--------------------	|---------	|----------------------	|----------------------	|---------------------------	|
| 01 	    | Regular            	| -Car    	| stem + ar            	| stem + ó             	| hablar ==> habló          	|
| 02 	    | Regular            	| -Var    	| stem + ar            	| stem + ó             	| pasear ==> paseó          	|
| 03 	    | Regular            	| -Cer     	| stem + er            	| stem + ió            	| comer ==> comió           	|
| 04 	    | Regular            	| -Cir     	| stem + ir            	| stem + ió            	| abrir ==> abrió           	|
| 05 	    | Regular            	| -guir   	| stem + guir          	| stem + guió          	| distinguir ==> distinguió 	|
| 06 	    | Regular            	| -quir   	| stem + quir          	| stem + quió          	| delinquir ==> delinquió   	|
| 07  	    | ñ-ending Stems     	| -ñer    	| stem(S, ñ) + er      	| stem(S, ñ) + ó       	| tañer ==> tañó            	|
| 08  	    | ñ-ending Stems     	| -ñir    	| stem(S, ñ) + ir      	| stem(S, ñ) + ó       	| gruñir ==> gruñó          	|
| 09  	    | Vowel-ending Stems 	| -Ver    	| stem(S, V) + er  	    | stem(S, V) + yó  	    | leer ==> leyó             	|
| 10    	| Vowel-ending Stems 	| -Vir    	| stem(S, V) + ir  	    | stem(S, V) + yó  	    | construir ==> construyó   	|
| 11  	    | Vowel Raising      	| -eCir   	| stem(S, e, C) + ir   	| stem(S, i, C) + ió   	| pedir ==> pidió           	|
| 12  	    | Vowel Raising      	| -eCCir  	| stem(S, e, CC) + ir  	| stem(S, i, CC) + ió  	| sentir ==> sintió         	|
| 13  	    | Vowel Raising      	| -eCCCir 	| stem(S, e, CCC) + ir 	| stem(S, i, CCC) + ió 	| henchir ==> hinchió       	|
| 14    	| Vowel Raising      	| -eguir  	| stem + eguir         	| stem + iguió         	| seguir ==> siguió         	|
| 15    	| Vowel Raising      	| -eñir   	| stem(S, e, ñ) + ir   	| stem(S, i, ñ) + ó    	| heñir ==> hiñó            	|

Here is a graph of the Spanish verb Finite State Transducer.

![Spanish verb Finite State Transducer](https://user-images.githubusercontent.com/19893082/196309421-c165fa2c-c0c8-4166-98df-7699e06d92d1.png)