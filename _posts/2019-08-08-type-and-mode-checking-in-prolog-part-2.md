---
layout: post
title: "Type and mode checking in prolog (part 2): mavis"
date: 2019-08-08
description:
cover_image: /assets/images/cover-image-03.jpeg
author: Gavin Mendel Gleason
tags:
  - Type Theory
  - Prolog
  - Determinism

---
As we discussed in [Part 1](/2019/06/07/type-and-mode-checking-in-prolog-part-1/) of this short series on types in prolog, prolog has a rather graceful but silent method of reacting to a certain bugs. The exploitation of non-determinism can be extremely handy for searching spaces, but sometimes we just didn’t mean what we wrote, and instead of silent failure we would like to see an error.

One way around this problem which we presented in Part 1 was to use Hindley-Milner type (HM) checking to statically discover where we have variables which have a different type than we expect.

We’re going to take a different tack in this article and use dynamic type and mode checking with mavis. HM is great, but it wasn’t really written for prolog but for functional languages, and so doesn’t take any account of modes. It can also be a bit restrictive and heavy weight when modifying already existing code. We could of course dial it up, by introducing dependent types, but we’re not going to go there in this series (though how cool would this be?!).

With [Mavis](https://github.com/GavinMendelGleason/mavis) we get to specify both modes and quite complex types in a gradual fashion, adding them as necessary. The downside is that type checking happens only at run-time and is not statically checked. Unfortunately this means we only catch bugs during testing or deployment, but at least we catch them!

I’ve used both techniques in production code and find them both useful, but currently Mavis is more fully featured and more flexible and so I’ve done most development with it.

What is a mode? Prolog is a language of predicates, and we can think of any given substitution of variables as being either true or false at a predicate. But often times what we are actually doing when programming in prolog is taking what we know about some bound variables, in order to find other ones which would also satisfy the predicate. And very often we mean a predicate to only search for bindings given some set of already instantiated variables.

Take for instance the following programme:

```prolog
/**
 * zip(+A:list(any),+B:list(any),-C:list(any)) is det.
 * zip(-A:list(any),-B:list(any),+C:list(any)) is det.
 *
 * Zip two lists into a list of pairs (or unzip, in the other two
 * modes)
 */
zip([A|RestA],[B|RestB],[(A-B)|Zip]) :-
  zip(RestA,RestB,Zip).
zip([],[],[]).
```

The benefit of this declaration is obvious from the results of querying for the following goal:

```prolog
?- zip([1,2,3],a,C).
ERROR: Unhandled exception: The term a is not in the domain list(any)
```
We have effectively stopped zip from silently failing in the event of passing nonsense arguments. But what do all of these complicated mode descriptions mean?

There are two mode-lines given for the above programme. We use a preceding ‘+’ to denote required arguments and ‘-’ to denote supplied arguments. If an argument is required (‘+’), it must be bound at the time the predicate is called. If it is supplied (‘-’), it can be bound or not, but we will guarantee that we supply a binding if it is unbound.

In fact binding time status is a bit more complicated than this. It’s quite possible to have a ‘skeleton’ which has unbound variables inside of a defined term. For instance, you can obtain such a skeleton of a list with the goal:

```prolog
?- length(L,3).
L = [_6186, _6192, _6198].
```

Is this `+`? Mavis interprets it as bound if it is compatible with the type declaration. To require a completely defined input you can use `++`. In the future we want to include more specific user-defined designations modeling ourselves after those given in Mercury.

![Mercury Diagram](/blog/assets/images/inner-image-03.png)

The final part of the mode describes the amount of determinism that the predicate is expected to have. Zip above is considered to be ‘det’ which means in all the modes that are described, and subject to its inputs being well typed, it will return precisely one substitution. The different determinism qualifiers are listed in the mavis documentation as follows:

* failure: 0 solutions
* semidet: 0 or 1 solution
* det: 1 solution
* multi: more than one solution
* nondet: Any number of solutions including 0

There are *allowable* modes which are *covered* by the above given modes to make things a little more confusing. For instance, I can call zip with the following two different goals, both of which are accepted as legitimate and do not yield errors.

```prolog
?- zip([1,2,3],[4,5,6],[1–4, 2–5, 3–6]).
true.
?- zip([1,2,3],[4,5,6],[1–4, 2–5, 3–7]).
false.
```

In the first case we supplied all of the arguments. This is more informative than any of the modes given, and consequently causes the determinsm to be *demoted*. We shift from being ‘det’ to being ‘semidet’. This demotion is taken care of for us automatically by mavis.
We can also run the query *backwards*, since we have supplied the appropriate mode line above. This is the main reason we might want to specify multiple mode lines.

```prolog
?- zip(A,B,[1–4, 2–5, 3–6]).
A = [1, 2, 3],
B = [4, 5, 6].
```

Mavis is really useful for finding bugs in production code during testing. I’ve recovered a large number of confusions, especially in the case of errors in my own thinking regards the determinism of various predicates. This can in turn have huge impacts on performance, since uncontrolled and unintended non-determinism can have surprisingly large impacts on speed.

There are however a number of things I’m unhappy with and I would like to see improved. I’ll list them out here as a sort of wish-list for mavis in the future.

* There should be a good deal more that is done statically. Many of the mode and type declarations give plenty of information to yield compile time errors.
* With a bit more declarative work we could even reordered queries automatically to produce much more desirable outcomes for specific modes — and in some cases we could even improve the termination behaviour!
* The types should at least give us the flexibility of Hindley-Milner type declarations with free type variables which can be used repeatedly. This would give us a much more pleasing and specific type for ‘zip’.
* Someday in the not too distant future it would be nice to see dependent types making their way in. For the dynamic case, as we currently have with mavis, this shouldn’t even be too terribly hard to implement in a hacky sort of way!

You can check out the lastest Mavis source here: [https://github.com/GavinMendelGleason/mavis](https://github.com/GavinMendelGleason/mavis)
