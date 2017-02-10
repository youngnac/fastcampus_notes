##Field options
###Null
-
- True : store empty as NULL in db (default: False)
- Do NOT use `NULL` on strings (storing NULL and empty string together)
- SET `blank=True` for strings and non-string-based fields if you want to allow empty values in froms
- for boolean: use `NullBooleanField`

###blank
-
- True: allowed to be blank (d: False)
- null > db-related vs. blank > validation-related
- True: form validation allows empty values
- False: requires field

###choices
-
- iteralbe (list/tuple) consisting itself of iterables of exactly TWO items (e.g. [(A, B), (A, B) ...])
- if given: by default, widget is a select box
- (A,B) 
	* A: actual value to be set on the model
	* B: human readable anme
- 

