---
title: Exceptions for flow control
tags:
    - Programming
---

Recently I was having a conversation with a very good friend about exceptions for flow control. 
I was against using exceptions and he was in favor of using them.

Generally, most people are against, as outlined in [c2.com](http://c2.com/cgi/wiki?DontUseExceptionsForFlowControl):

> Exceptions (in Java and C++) are like non-local goto statements. As such they can be used to build general control flow constructs. For example, you could write a recursive search for a binary tree which used an exception to return the result:
>
>       void search( TreeNode node, Object data ) throws ResultException {
>           if (node.data.equals( data ))
>               throw new ResultException( node );
>           else {
>               search( node.leftChild, data );
>               search( node.rightChild, data );
>           }
>       }

or in this q/a from [stack overflow](http://stackoverflow.com/a/729404):

> Exceptions are basically non-local goto statements with all the consequences of the latter. Using exceptions for flow control violates a principle of least astonishment, make programs hard to read (remember that programs are written for programmers first).
>
> Moreover, this is not what compiler vendors expect. They expect exceptions to be thrown rarely, and they usually let the throw code be quite inefficient. Throwing exceptions is one of the most expensive operations in .NET.
> 
> However, some languages (notably **Python**) use exceptions as flow-control constructs. For example, iterators raise a StopIteration exception if there are no further items. Even standard language constructs (such as for) rely on this.

Did I mention my friend is a pythonista? This explains a lot. 

Of course some are in favor, like this [article](http://www.drmaciver.com/2009/03/exceptions-for-control-flow-considered-perfectly-acceptable-thanks-very-much/):

> Apparently the fact that I use exceptions in my suggested control flow mechanism for collections makes me a bad person. This idiocy touches on a sore point for me. I have been invited to write a post on “Why exceptions for control flow are a good idea”. So. Here it is.
>
> The thing is, I really can’t be bothered. I can give and have given useful use cases, but I’ll just be told that they’re evil because I use exceptions. So instead I will argue the converse: Why they are not a bad idea.

My position is that exceptions are exceptional and should be used to signal about exceptional situations. 
Like hardware exceptions. Let me give you an example with a DateRange class:

    <?php
    /**
     * This is a demonstration class that stores two datetime objects
     */
    class DateRange
    {
        /**
         * @var DateTime
         */
        private $start;
        
        /**
         * @var DateTime
         */
        private $end;
        
        /**
         * DateRange constructor.
         *
         * @param DateTime $start
         * @param DateTime $end
         */
        public function __construct(DateTime $start, DateTime $end)
        {
            $this->start = $start;
            $this->end   = $end;
        }

        /**
         * @return DateTime
         */
        public function start()
        {
            return $this->start;
        }
        
        /**
         * @return DateTime
         */
        public function end()
        {
            return $this->end;
        }
        
        /**
         * @param string $format
         *
         * @return string
         */
        public function diff($format)
        {
            return $this->end->diff($this->start)->format($format);
        }
    }

## Using Exceptions

We need to be sure that the `$start` and `$end` objects are in order, so we modified the constructor:

    public function __construct(DateTime $start, DateTime $end)
    {
        if ($start >= $end) { 
            throw new Exception("Datetime objects not in order");
        }        
        $this->start = $start;
        $this->end   = $end;
    }
    
and we can use the class like this:

    <?php
    $start = new DateTime("2015-10-10");
    $end = new DateTime("2015-10-12");
    try {
        $range = new DateRange($start, $end);
    } catch(Exception $e) {
        // handle exception...
    }

## Using Validation

The same can be achieved by adding this method to the class:

    public static function valid(DateTime $start, DateTime $end)
    {
        return $start < $end;
    }
    
and modifying the constructor like this:

    public function __construct(DateTime $start, DateTime $end)
    {
        if (!self::valid($start, $end)) {
            throw new \Exception("Datetime objects not valid");
        }
        $this->start = $start;
        $this->end   = $end;
    }
    
and now we can use the class like this (using the [data guard](http://c2.com/cgi/wiki?GuardClause) pattern):

    <?php
    $start = new DateTime("2015-10-10");
    $end = new DateTime("2015-10-12");
    if (!DateRange::valid($start, $end)) {
        // handle situation, eg return, exit, throw an exception
    }
    $range = new DateRange($start, $end);

Notice that I still use the exception in the constructor, because you can't allow the creation of the object
with invalid data. What I did is I updated the API in order to give the developer the option of validating
the input data before creating the object.

## Conclusion

I prefer to use the classical control structures for flow control and exceptions for situations where something
could not execute normally (eg a network error on an API request). If some library does the opposite, ok, no big deal. 
But see above for yourself, which one is more elegant?


