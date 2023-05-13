Get a brief introduction to "Netflix" and learn which features we'll be building in this project.

We'll cover the following

#### Table of Content
- [Introduction](#introduction)
- [Statement](#statement)
- [Features](#features)
- [Feature 1 Group Similar Titles](#feature-1-group-similar-titles)
  - [Description](#description)
  - [Solution](#solution)
  - [Feature-1-Code](#feature-1-code)
  - [Complexity measures](#complexity-measures)
    - [Time Complexity](#time-complexity)
    - [Space complexity](#space-complexity)
- [Feature 2 Fetch Top Movies](#feature-2-fetch-top-movies)
  - [Description](#description-1)
  - [Solution](#solution-1)
  - [Feature-2-Code](#feature-2-code)
  - [Complexity measures](#complexity-measures-1)
    - [Time Complexity](#time-complexity-1)
    - [Space complexity](#space-complexity-1)
- [Feature 3 Find Median Age](#feature-3-find-median-age)
  - [Description](#description-2)
  - [Solution](#solution-2)
  - [Feature-3-Code](#feature-3-code)
  - [Complexity measures](#complexity-measures-2)
    - [Time Complexity](#time-complexity-2)
    - [Memory complexity](#memory-complexity)


## [Introduction](#Introduction)

_Netflix_ is the biggest video streaming platform in the world, offering movies, seasons, biographies, reality shows, and more. Their video repository is growing significantly. So the engineering team at _Netflix_ keeps trying to find better ways to display content to their consumers.

The scenario and the problems discussed in this chapter also relate to any content displaying functionality and how we can improve it.

## [Statement](#Statement)

Let’s pretend you’re a developer on the _Netflix_ engineering team. You are working on improving the user experience in finding content to watch. This involves the improvement of the search as well as recommendation functionality.


## [Features](#Features)

We will need to introduce the following features to implement the improvements discussed above:

-  [**Feature # 1:**](#feature-1-group-similar-titles) We want to enable users to see relevant search results despite minor typos.
    
-   [**Feature # 2:**](#feature-2-fetch-top-movies) Enable the user to view the top-rated movies worldwide, given that we have movie rankings available separately for different geographic regions.
    
-   [**Feature # 3:**](#feature-3-find-median-age) As part of a demographic study, we are interested in the median age of our viewers. We want to implement a functionality whereby the median age can be updated efficiently whenever a new user signs up for Netflix.
    
-   [**Feature # 4:**]() For efficiently distributing content to different geographic regions and for program recommendation to viewers, we want to determine titles that are gaining or losing popularity scores.
    
-   [**Feature # 5:**]() For the client application, we want to implement a cache with a replacement strategy that replaces the least recently watched title.
    
-   [**Feature # 6:**]() As an alternative to the above, we want to implement a strategy of replacing the least frequently watched titles in the cache.
    
-   [**Feature # 7:**]() During a user session, a user often “shops” around for a program to watch. During this session, we want to let them move back and forth in the history of programs they’ve just browsed. As a developer, you can smell a _stack_, right? But, we also want the user to be able to directly jump to the top-ranked program from the one’s they’ve browsed.
    
-   [**Feature # 8:**]() As you beta tested [**feature #7**](), a user complained that the _next_ and _previous_ functionality isn’t working correctly. Using their session history, we want to check if our implementation is correct or indeed buggy.
    
-   [**Feature # 9:**]() Given an initial set of categories/genres, we want to implement a functionality that returns all the possible movie combinations of a particular category/genre.
    
-   [**Feature # 10:**]() During a user session, there is often a possibility that issues like packet drops and buffering events might emerge. We want to record the median number of buffering events that might occur in a particular session, which could then be used to improve the user experience.
    
-   [**Feature # 11:**]() A set of movies needs to be presented in different orders to a different set of users. We want to generate all the possible permutations of movies in a given marathon.
    
-   [**Feature # 12:**]() For enhancing the Netflix user experience, we want to revamp the continue watching bar that will return the most frequently watched show.
    

Understanding these feature requests and designing their solutions will help us implement the requested functionality into Netflix’s system.

Before we start, Think about how you would implement these features. As you do, you’ll realize some of the underlying problems that you’ll need to solve.

<hr>
<details><summary>Feature 1 Group Similar Titles</summary>
<p>
## Feature 1 Group Similar Titles
Implementing the "Group Similar Titles" feature for our "Netflix" project.

### Description
First, we need to figure out a way to individually group all the character combinations of each title. Suppose the content library contains the following titles: `"duel", "dule", "speed", "spede", "deul", "cars"`. How would you efficiently implement a functionality so that if a user misspells speed as spede, they are shown the correct title?

We want to split the list of titles into sets of words so that all words in a set are anagrams. In the above list, there are three sets: {"duel", "dule", "deul"}, {"speed", "spede"}, and {"cars"}. Search results should comprise all members of the set that the search string is found in. We should pre-compute these sets instead of forming them when the user searches a title.

Here is an illustration of this process:
![Combining similar groups](./pics/combining%20similar%20groups.png)


### Solution
From the above description, we see that all members of each set are characterized by the same frequency of each alphabet. This means that the frequency of each alphabet in words belonging to the same group is equal. In the set `{{"speed", "spede"}}`, the frequency of the characters s, p, e, and d are the same in each word.

Let’s see how we might implement this functionality:

For each title, compute a 26-element vector. Each element in this vector represents the frequency of an English letter in the corresponding title. This frequency count will be represented as a string delimited with # characters. For example, `abbccc` will be represented as `#1#2#3#0#0#0...#0`. This mapping will generate identical vectors for strings that are anagrams.

Use this vector as a key to insert the titles into a Hash Map. All anagrams will be mapped to the same entry in this Hash Map. When a user searches a word, compute the 26-element English letter frequency vector based on the word. Search in the Hash Map using this vector and return all the map entries.

Store the vector of the calculated character counts in the same Hash Map as a key and assign the respective set of anagrams as its value.
Return the values of the Hash Map, since each value will be an individual set.
Let’s look at the following illustration to clarify this process:
<img src="./pics/Storing%20sets%20in%20a%20key-value%20storage.png" width="60%" height="60%"/>

Let’s look at the code for the solution below:


### [Feature-1-Code](https://github.com/ani2fun/java-kotlin-journal/blob/main/java-playground/src/main/java/io/journal/sysdesign/tutorials/netflixing/feature_1/Solution.java)
### Complexity measures

| **Time Complexity** | **Space complexity** |
| --------------- | ---------------- |
| O(n×k)          | O(n×k)           |

Let n be the size of the list of strings, and k be the maximum length that a single string can have.

#### Time Complexity
We are counting each letter for each string in a list, so the time complexity will be O(n×k).

#### Space complexity
Since every string is being stored as a value in the dictionary whose size can be n, and the size of the string can be k, so space complexity is O(n×k).

</p>

</details>

<hr>

<details><summary>Feature 2 Fetch Top Movies</summary>

## Feature 2 Fetch Top Movies

Implementing the "Fetch Top Movies" feature for our "Netflix" project.

### Description
Now, we need to build a criterion so the top movies from multiple countries will combine into a single list of top-rated movies. In order to scale, the content search is performed in a distributed fashion. Search results for each country are produced in separate lists. Each member of a given list is ranked by popularity, with 1 being most popular and popularity decreasing as the rank number increases.

Let’s say that the following titles are represented by the provided IDs:

<img src="./pics/Movie%20mapping%20to%20their%20ranks.png"  width="40%" height="30%">

We’ll be given n arrays that are all sorted in ascending order of popularity rank. We have to combine these lists into a single list that will be sorted by rank in ascending order, meaning from best to worst.

> Keep in mind that the ranks are unique to individual movies and a single rank can be in multiple lists.

Let’s understand this better with an illustration:

<img src="./pics/Combining%20lists%20of%20multiple%20ratings%20into%20one.png"  width="100%" height="30%">

### Solution
Since our task involves multiple lists, you should divide the problem into multiple tasks, starting with the problem of combining two lists at a time. Then, you should combine the result of those first two lists with the third list, and so on, until the very last one is reached.

Let’s discuss how we will implement this process:

Consider the first list as the result, and store it in a variable.
Traverse the list of lists, starting from the second list, and combine it with the list we stored as a result. The result should get stored in the same variable.
When combining the two lists, like `l1` and `l2`, maintain a prev pointer that points to a dummy node.
If the value for list `l1` is less than or equal to the value for list `l2`, connect the previous node to `l1` and increment `l1`. Otherwise, do the same but for list `l2`.
Keep repeating the above step until one list points to a null value.
Connect the non-null list to the merged one and return.
Let’s look at the code for the solution below:

### [Feature-2-Code](https://github.com/ani2fun/java-kotlin-journal/blob/main/java-playground/src/main/java/io/journal/sysdesign/tutorials/netflixing/feature_2/_Main.java)


### Complexity measures

| **Time Complexity** | **Space complexity** |
| ------------------- | -------------------- |
| O(n×k<sup>2</sup>)  | O(1)                 |

#### Time Complexity
The time complexity will be O(n×k<sup>2</sup>), where k is the number of the lists and n is the maximum length of a single list.

#### Space complexity 
O(1) , as constant space was utilized.

</details><hr>

<details>
<summary>Feature 3 Find Median Age</summary>
## Feature 3 Find Median Age
Implementing the "Find Median Age" feature for our "Netflix" project.

### Description

Our third task is building a filter that will fetch relevant content based on the age of the users for a specific country or region. For this, we use the median age of users and the preferred content for users that fall into that specified category.

Because the number of users is constantly increasing on the Netflix platform, we need to figure out a structure or design that updates the median age of users in real-time. We will have an array that constantly receives age values, and we will output the median value after each new data point.

Let’s understand this better with an illustration:
<div>
    <img src="./pics/Median%20Age-1.png"  width="50%" height="30%">
</div>
<div>
    <img src="./pics/Median%20Age-2.png"  width="50%" height="30%">
</div>

### Solution
We will assume that ‘x’ is the median age of a user in a list. Half of the ages in the list will be smaller than (or equal to) ‘x’, and the other half will be greater than (or equal to) ‘x’. We can divide the list into two halves: one half to store the smaller numbers (say smallList), and one half to store the larger numbers (say largeList). The median of all ages will either be the largest number in the smallList or the smallest number in the largeList. If the total number of elements is even, we know that the median will be the average of these two numbers. The best data structure for finding the smallest or largest number among a list of numbers is a Heap.

Here is how we will implement this feature:

First, we will store the first half of the numbers (smallList) in a Max Heap. We use a Max Heap because we want to know the largest number in the first half of the list.
Then, we will store the second half of the numbers (largeList) in a Min Heap, because we want to know the smallest number in the second half of the list.
We can calculate the median of the current list of numbers using the top element of the two heaps.
Let’s look at the code for this solution below:


### [Feature-3-Code](https://github.com/ani2fun/java-kotlin-journal/blob/main/java-playground/src/main/java/io/journal/sysdesign/tutorials/netflixing/feature_3/MedianOfAges.java)


### Complexity measures

| **Time Complexity** | **Memory complexity**|
| ------------------- | -------------------- |
| Insert Age: O(logn) | O(n)                 |
| Find Median: O(1)   |                      |

#### Time Complexity
The time complexity of the Insert Age will be O(logn) because we inserted in the heap. The time complexity of the Find Median will be O(1) because we can find the median from the top elements of the heaps.

#### Memory complexity
The memory complexity will be O(n) because we will be storing all the numbers at any time.

</details><hr>

