Get a brief introduction to "Netflix" and learn which features we'll be building in this project.

We'll cover the following

#### Table of Content
- [Introduction](#introduction)
- [Statement](#statement)
- [Features](#features)
- [Feature 1 Group Similar Titles](#feature-1-group-similar-titles)
  - [Description](#description)
  - [Solution](#solution)
  - [Complexity measures](#complexity-measures)
    - [Time Complexity](#time-complexity)
    - [Space complexity](#space-complexity)
- [Feature 2 Fetch Top Movies](#feature-2-fetch-top-movies)
  - [Description](#description-1)
  - [Solution](#solution-1)
  - [Complexity measures](#complexity-measures-1)
    - [Time Complexity](#time-complexity-1)
    - [Space complexity](#space-complexity-1)


## [Introduction](#Introduction)

_Netflix_ is the biggest video streaming platform in the world, offering movies, seasons, biographies, reality shows, and more. Their video repository is growing significantly. So the engineering team at _Netflix_ keeps trying to find better ways to display content to their consumers.

The scenario and the problems discussed in this chapter also relate to any content displaying functionality and how we can improve it.

## [Statement](#Statement)

Let’s pretend you’re a developer on the _Netflix_ engineering team. You are working on improving the user experience in finding content to watch. This involves the improvement of the search as well as recommendation functionality.


## [Features](#Features)

We will need to introduce the following features to implement the improvements discussed above:

-  [**Feature # 1:**](#feature-1-group-similar-titles) We want to enable users to see relevant search results despite minor typos.
    
-   [**Feature # 2:**](#feature-2-fetch-top-movies) Enable the user to view the top-rated movies worldwide, given that we have movie rankings available separately for different geographic regions.
    
-   [**Feature # 3:**]() As part of a demographic study, we are interested in the median age of our viewers. We want to implement a functionality whereby the median age can be updated efficiently whenever a new user signs up for Netflix.
    
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
![](./pics/Storing%20sets%20in%20a%20key-value%20storage.png)

Let’s look at the code for the solution below:
```java
import java.util.*;
class Solution {
    public static List<List<String>> groupTitles(String[] strs){
      if (strs.length == 0) 
            return new ArrayList<List<String>>();

        Map<String, List<String>> res = new HashMap<String, List<String>>();

        int[] count = new int[26];
        for (String s : strs) {
            Arrays.fill(count, 0);
            for (char c : s.toCharArray()){
                int index = c - 'a';
                count[index]++;
            }

            StringBuilder delimStr = new StringBuilder("");
            for (int i = 0; i < 26; i++) {
                delimStr.append('#');
                delimStr.append(count[i]);
            }
            
            String key = delimStr.toString();
            if (!res.containsKey(key)) 
                res.put(key, new ArrayList<String>());
            
            res.get(key).add(s);
        }

        return new ArrayList<List<String>>(res.values());
    }

    public static void main(String[] args) {
        // Driver code
        String titles[] = {"duel","dule","speed","spede","deul","cars"};

        List<List<String>> gt = groupTitles(titles);
        String query = "spede"; 

        // Searching for all titles
        for (List<String> g : gt){
            if (g.contains(query))
                System.out.println(g);
        }
  }
}

/**
 * 
 * Output
[speed, spede]
 * */ 

```

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


`Main.java`
```java
class MergeSortList{
  public static LinkedListNode merge2Country(LinkedListNode l1, LinkedListNode l2) {
    LinkedListNode dummy = new LinkedListNode(-1);

    LinkedListNode prev = dummy;
    while (l1 != null && l2 != null) {
        if (l1.data <= l2.data) {
            prev.next = l1;
            l1 = l1.next;
        } else {
            prev.next = l2;
            l2 = l2.next;
        }
        prev = prev.next;
    }
    
    if (l1 == null)
      prev.next = l2;
    else
      prev.next = l1;

    return dummy.next;
  }
  
  public static LinkedListNode mergeKCounty(List<LinkedListNode> lists) {

    if (lists.size() > 0){
      LinkedListNode res = lists.get(0);

      for (int i = 1; i < lists.size(); i++)
        res = merge2Country(res, lists.get(i));

      return res;
    }
    return new LinkedListNode(-1);
  }

  public static void main(String[] args) {

    LinkedListNode a = LinkedList.createLinkedList(new int[] {11,41,51});

    LinkedListNode b = LinkedList.createLinkedList(new int[] {21,23,42});

    LinkedListNode c = LinkedList.createLinkedList(new int[] {25,56,66,72});
    
    List<LinkedListNode> list1 = new ArrayList<LinkedListNode>();
    list1.add(a);
    list1.add(b);
    list1.add(c);

    System.out.print("All movie ID's from best to worse are:\n");
    LinkedList.display(mergeKCounty(list1));
  }
}

/*
Output
All movie ID's from best to worse are:
11, 21, 23, 25, 41, 42, 51, 56, 66, 72
*/
```

`LinkedList.java`

```java
import java.util.*;

class LinkedListNode {
    public int key;
    public int data;
    public LinkedListNode next;
    public LinkedListNode arbitraryPointer;

    public LinkedListNode(int data) {
        this.data = data;
        this.next = null;
    }

    public LinkedListNode(int key, int data) {
        this.key = key;
        this.data = data;
        this.next = null;
    }

    public LinkedListNode(int data, LinkedListNode next) {
        this.data = data;
        this.next = next;
    }

    public LinkedListNode(int data, LinkedListNode next, LinkedListNode arbitraryPointer) {
        this.data = data;
        this.next = next;
        this.arbitraryPointer = arbitraryPointer;
    }
}



class LinkedList { 

    public static LinkedListNode insertAtHead(LinkedListNode head, int data) {
        LinkedListNode newNode = new LinkedListNode(data);
        newNode.next = head;
        return newNode;
    }

    public static LinkedListNode insertAtTail(LinkedListNode head, int data) {
        LinkedListNode newNode = new LinkedListNode(data);
        if (head == null) {
            return newNode;
        }
        LinkedListNode temp = head;
        while (temp.next != null) {
            temp = temp.next;
        }
        temp.next = newNode;
        return head;
    }

    public static LinkedListNode insertAtTail(LinkedListNode head, LinkedListNode node)
    {
        if (head == null) {
            return node;
        }
        LinkedListNode temp = head;
        while (temp.next != null) {
            temp = temp.next;
        }
        temp.next = node;
        return head;
    }

    public static LinkedListNode createLinkedList(ArrayList<Integer> lst) {
        LinkedListNode head = null;
        LinkedListNode tail = null;
        for (Integer x : lst) {
            LinkedListNode newNode = new LinkedListNode(x);
            if (head == null) {
                head = newNode;
            } else {
                tail.next = newNode;
            }
            tail = newNode;
        }
        return head;
    }

    public static LinkedListNode createLinkedList(int[] arr) {
        LinkedListNode head = null;
        LinkedListNode tail = null;
        for (int i = 0; i < arr.length; ++i) {
            LinkedListNode newNode = new LinkedListNode(arr[i]);
            if (head == null) {
                head = newNode;
            } else {
                tail.next = newNode;
            }
            tail = newNode;
        }
        return head;
    }

    public static LinkedListNode createRandomList(int length) {
        LinkedListNode listHead = null;
        Random generator = new Random();
        for(int i = 0; i < length; ++i) {
            listHead = insertAtHead(listHead, generator.nextInt(100));
        }
        return listHead;
    }

    public static ArrayList<Integer> toList(LinkedListNode head) {
        ArrayList<Integer> lst = new ArrayList<Integer>();
        LinkedListNode temp = head;
        while (temp != null) {
            lst.add(temp.data);
            temp = temp.next;
        }
        return lst;
    }

    public static void display(LinkedListNode head) {
        LinkedListNode temp = head;
        while (temp != null) {
            System.out.printf("%d", temp.data);
            temp = temp.next;
            if (temp != null) {
              System.out.printf(", ");
            }
        }
        System.out.println();
    }  
    

    public static LinkedListNode mergeAlternating(LinkedListNode list1, LinkedListNode list2) {
      if (list1 == null) {
        return list2;
      }

      if (list2 == null) {
        return list1;
      }

      LinkedListNode head = list1;

      while (list1.next != null && list2 != null) {
        LinkedListNode temp = list2;
        list2 = list2.next;

        temp.next = list1.next;
        list1.next = temp;
        list1 = temp.next;
      }

      if (list1.next == null) {
        list1.next = list2;
      }

      return head;
    }

    static boolean isEqual(LinkedListNode list1, LinkedListNode list2) {
        if (list1 == list2) {
            return true;
        }

        while (list1 != null && list2 != null) {
            if (list1.data != list2.data) {
                return false;
            }

            list1 = list1.next;
            list2 = list2.next;
        }

        return (list1 == list2);
    }
}
```

### Complexity measures

| **Time Complexity** | **Space complexity** |
| ------------------- | -------------------- |
| O(n×k<sup>2</sup>)  | O(1)                 |

#### Time Complexity
The time complexity will be O(n×k<sup>2</sup>), where k is the number of the lists and n is the maximum length of a single list.

#### Space complexity 
O(1) , as constant space was utilized.

</details>

