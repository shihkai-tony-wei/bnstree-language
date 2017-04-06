# bnstree-language

This is a json dump of all the language data on bnstree.


# About { } brackets

BnSTree works by filling in values and combining templates/clauses to create a sentence. The { } brackets define where the special values or sentence clauses go and are filled in by the data being passed in. 

For example:
```javascript
{
    "template" : "TEMPLATE_INFLICT_STATUS", 
    "variables" : {
        "status" : "knockdown", 
        "time" : 3,
        "withinArea" : true
    }
}
```
The data above creates a sentence with `TEMPLATE_INFLICT_STATUS` which looks like...
```javascript
{$skill} inflicts {#stack=CLAUSE_STACK} {#time=CLAUSE_TIME} {$status} {#statDown=CLAUSE_STAT_DOWN} {#timeAfter=CLAUSE_TIME_AFTER} {&withinArea=to enemies within the area} {&players=to players and familiars} {&additional=to additional enemies hit} {#condition=CLAUSE_CONDITION_ON} {#statusOther=CLAUSE_TO_STATUS}
```

Crazy yes.. But if you look at `variables`, you can see that it only has `status` and `time`. The parser will cut out all the unused brackets which will then look like...
```javascript
inflicts {#time=CLAUSE_TIME} {$status} {&withinArea=to enemies within the area}
```
Much better. Now if you take a closer look, you can see that the brackets begin with a certain symbol.

#### $ 
Brackets with this symbol are replaced by a simple value or a list of comma separated values. The value can also be a reference to another variable. In our example, `{$status}` will be replaced with `knock down` (`knockdown` is defined in `skillConstants.STATUS`)

#### \#
Brackets with this symbol are replaced replaced with another template. In other words, the parser can recursively add in clauses/templates as well. In our example, we have `{#time=CLAUSE_TIME}`. Here, `CLAUSE_TIME` is the default template that it's going to fill the spot (but note that this can be overridden by the data)

```javascript
{$time} sec
```

Following our previous rule, this becomes `3 sec`.

#### &
This is a flag. If the variable is `true`, then add in the words after the `=` sign. So, in our example, since `withinArea` is true, the bracket will be replaced with `to enemies within the area`.


#### ?
This is a query and it looks something like...
```javascript
{?x; value>1; a; b}
```
Although it's not in our current example, this basically means if the value of `x` is `>1`, replace the bracket with `a`. Else `b`.

Now once we have all the brackets parsed, then it is just the matter of filling in the blanks. 
```javascript
inflicts 3 sec knock down to enemies within the area
```

---


As a translator, most of the effort would be trying to reorganize the brackets so that the sentence it creates is coherent for 'most' cases. Most skill descriptions in BnS follow a similar structure (at least in English and Korean) so I recommend you devise a general rule and stick to it.



# Contribute~

If you would like to add your language to bnstree, feel free to fork this repository to your own GitHub accound and make your changes.

Remember to use the correct two letter [language code](https://www.w3schools.com/tags/ref_language_codes.asp) when creating new fields.
