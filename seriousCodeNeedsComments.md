# Serious code needs comments 

## adapted from http://www.reddit.com/r/ProgrammerHumor/comments/316aax/theres_always_that_one_guy_in_your_team/cpyv806

__Bob__

Serious code need comments. Not all code is serious.

__Zach__

Rubish. All comments are unnessary. Your code should be self documenting.

__Bob__

Sure some code can do with out comments, like UI code that will be thrown away in 6 months. Or extremely simple code.

A comment like this is a good reason to stab someone with a rusty fork:

    /** return the postal code */
    String getPostalCode ()
    
But serious code is not that. Serious stuff like  compute, transform, validate etc. Serious code needs context.
  
__Zach__

I'm pretty sure I disagree with you whole-heartedly. Im confident if you give me some serious codes i can eliminate
all your comments.

__Bob__

I Found a real example. Algorythm documented there:
http://en.wikipedia.org/wiki/Social_Insurance_Number#Validation

     //see http://en.wikipedia.org/wiki/Social_Insurance_Number#Validation (usually french link)
    static boolean validateNAS(String Nas){
        def total = 0;
        def partial = 0;
        def nasBytes = Nas.getBytes()

        for(int i =0; i < nasBytes.size();i++){
            partial = nasBytes[i] - ('0' as char) //get int value
            partial *=  (i%2)?2:1 //multiply by 2 for odd index
            total += partial%10 + ((int)(partial/10)) //get digital root, and add them to total
        }
        return total%10 == 0 //result should be divisible by 10
    }
    
    Good luck understanding that code without comments...
    
__Zach__

Yikes! Even with comments thats tuff. 

What do you think of this

    static boolean validateNAS(String Nas){
        def total = 0;
        def nasBytes = Nas.getBytes()
        getOddBytesIntValue(nasBytes).each {
            total += getDigitalRoot(it)
        }
        getEvenBytesIntValue(nasBytes).each {
            total += getDigitalRoot(it * 2)
        }
        return total%10 == 0
    }

    static List<int> getOddBytesIntValue(def bytes) {
        def oddBytes = bytes.findAll {
            return !isEven(it);
        }
        return oddBytes.collect { charAsInt(it) }
    }

    static List<int> getEvenBytesIntValue(def bytes) {
        def evenBytes = bytes.collect {
            return isEven(it);
        }
        return evenBytes.collect {charAsInt(it) }
    }

    static int charAsInt(char c) {
        return c - ('0' as char);
    }

    static boolean isEven(int n) {
        return (i%2) ? true : false;
    }

    static int getDigitalRoot(int n) {
        return n%10 + ((int)(n/10))
    }
    
    
__Bob__

I like your code, but I don't see how its more clear. For one, its much bigger now.  I can't even read it all without scrolling. Also, your `validateNAS` function is confusing. In a code review I would change a few things... and __add comments__.

    static boolean validateNAS(String Nas){
        def total = 0;

        //sum odd number
        getOddBytesIntValue(Nas.getBytes()).each {
            total += it //digital root not require, a single digit digital root is itself
        }

        //sum digital root of even number * 2
        getEvenBytesIntValue(Nas.getBytes()).each {
            total += getDigitalRoot(it * 2)
        }

        return total%10 == 0 //should be divisible by 10
    }

__Zach__

Ok, I see your point. But I can fix it!

Very good feedback on clarity issues (and the bug catch). This is why code reviews (and pair programming) are good. Does this version read a bit better to you?

    static boolean validateNAS(String Nas){
        def nasBytes = Nas.getBytes()
        int checksum = getBytesAtOddIndexes(nasBytes).sum { intValue(it) }
        checksum += getBytesAtEvenIndex(nasBytes).sum{  digitalRoot(intValue(it) * 2) }
        return checksum % 10 == 0;
    }
    
__Bob__

Ok. I get were your comming from. This reads very well and doesn't need comments! Lets look at this one more time as a whole.

> Code Dump


 




  


