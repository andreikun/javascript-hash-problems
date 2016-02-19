# javascript-hash-problems

###Problem 1

Convert this string

    string = "{key:[[value_1, value_2],[value_3, value4]], 5:10:00AM]}"

to this hash:

    h = {"key" => [["value_1", "value_2"],["value_3", "value4"]], 5=>"10:00AM"}

then convert h to JSON.
Please note that the brackets are unbalanced on purpose.    
    
#### Solution    
    
    var hash = "{firstKey:value0, key:[[value_1, value_2],[value_3, value4]], 5:10:00AM]}";

	function hashify(jsonString) {
    	s = jsonString.replace(/(\s*?{\s*?|\s*?,\s*?)(['"])?([a-zA-Z0-9]+)(['"])?:/g, '$1"$3":'); //Add quotes for keys
  		s = s.replace(/(\d\d\:\d\d\w\w)\]/, '$1'); //remove trailing brackets.
  		s = s.replace(/:((?:(?![\["]).)[A-z0-9:]+)/g, ':"$1"'); //add quotes to every value except an array value
  		s = s.replace(/([a-z0-9+_]+?(?=[ \],]))/g, '"$1"'); //add quotes to every array value

		try{
    		var h = JSON.parse(s);
  		}catch(e){
  			console.log("Don't want to end up here!")
  		}
  
  		return h;
	}

	function stringify(object) {
		if (typeof object !== 'object') {
  			console.log('Don`t to this!');
  		}	
  
  		try{
    		var j = JSON.stringify(object);
  		}catch(e){
  			console.log("Don't want to end up here!")
  		}
  
  		return j;
	}

	var h = hashify(hash);
	var j = stringify(h);

	console.log(h);
	console.log(j);


### Problem 2

Write a class `Sample` whose initialize method takes an arbitrary hash, e.g.

	h = {"this" => [1,2,3,4,5,6], "that" => ['here', 'there', 'everywhere'], :other => 'here'}

and represent each key in the hash as an attribute of an instance of the class, such that I can say:

	c = Sample.new(h)

and then

	c.this` should return `[1,2,3,4,5,6]

	c.that`  should return `['here', 'there', 'everywhere']

	c.other` should return `’here’

#### Solution

	function Sample(obj) {
	    for(var key in obj) {
        	if (obj.hasOwnProperty(key)) {
          	    	this[key] = obj[key];
        		}
       		}
        }

    var sampleObj = new Sample({"this":[1,2,3,4,5,6], "that":['here', 'there', 'everywhere'], "other": 'here'});
