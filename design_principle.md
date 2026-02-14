Software Design Principles
Software design principles are guidelines that help software developers create systems that are easy to understand, maintain, and extend. These principles can be applied at both the high-level and low-level design stages. Here are three cornerstone key software design principles:

DRY
KISS
YAGNI

Let's understand each of these design principles in detail.
1. DRY — Don't Repeat Yourself
This principle states that every piece of knowledge must have a single, unambiguous, authoritative representation within a system. In simple terms, avoid duplication of logic or code. Repeating code makes the system hard to maintain and error-prone. If a change is required, you might forget to update all occurrences.

Importance:
Reduces redundancy
Easier maintenance
Single point of change
Example:
First, consider a bad example of code that violates the DRY principle:

C++


#include <bits/stdc++.h>
using namespace std;

int main() {
    int length1 = 10, width1 = 5;
    int area1 = length1 * width1;
    cout << "Area1: " << area1 << endl;

    int length2 = 8, width2 = 4;
    int area2 = length2 * width2;
    cout << "Area2: " << area2 << endl;

    return 0;
}



In the above code, the logic for calculating the area is repeated in both printArea1() and printArea2() methods. If we need to change the logic, we have to do it in multiple places.

Now let's refactor this code to follow the DRY principle:

C++


#include <bits/stdc++.h>
using namespace std;

class AreaCalculator {
public:
    static int calculateArea(int length, int width) {
        return length * width;
    }
};

int main() {
    int area1 = AreaCalculator::calculateArea(10, 5);
    int area2 = AreaCalculator::calculateArea(8, 4);

    cout << "Area1: " << area1 << endl;
    cout << "Area2: " << area2 << endl;

    return 0;
}



In this refactored code, we have created a single method calculateArea that calculates the area. Now, if we need to change the logic, we only need to do it in one place.
Applying DRY in Practice:
Identify repetitive code and replace it with a single, reusable code segment.
Extract common functionality into methods or utility classes.
Leverage libraries and frameworks when available.
Refactor duplicate logic regularly across classes or layers
When Not to Use the DRY Principle:
Premature Abstraction: Don't extract common code too early.
At first glance, two code blocks might look similar, but they could change in different ways later.
Extracting them into a shared method can create unnecessary coupling between unrelated parts.
Performance-Critical Code: Don't apply DRY to performance-sensitive code if it causes inefficiency.
Sometimes, repeating optimized low-level logic is faster than calling a generalized, reusable method.
Function calls, indirection, or generic wrappers might reduce performance or block compiler optimizations like inlining.
Sacrificing Readability: If extracting repeated code makes the code less readable, prefer clarity over DRYness.
Legacy Codebases: Don't refactor for DRY's sake in legacy code unless necessary and well-tested.
Legacy code might not have tests or complete documentation. Introducing DRY by extracting shared logic can accidentally change behavior.
Refactoring legacy code safely often follows the "leave it alone unless you must touch it" rule.
2. KISS — Keep It Simple, Stupid
This principle states that simplicity should be a key goal in design and unnecessary complexity should be avoided. In simple terms, use the simplest possible solution that works. Avoid clever, convoluted code.

Importance
Easier debugging
Improved readability
Better maintainability
Faster development
Example:
Assume that we are writing some piece of code to check if a number is even or not. Let's compare a bad (overengineered) vs. a good (simple and readable) example.

Bad Code (Too Complex):

C++


#include <bits/stdc++.h>
using namespace std;

class NumberUtils {
public:
    static bool isEven(int number) {
        // Using unnecessary logic to determine evenness
        bool isEven = false;

        if (number % 2 == 0) {
            isEven = true;
        } else {
            isEven = false;
        }

        return isEven;
    }
};



The above code is considered bad because it:
Uses extra variables.
Adds unnecessary if-else logic.
Makes the code longer and harder to follow.


Good Code (Simple and Clear):

C++


#include <bits/stdc++.h>
using namespace std;

class NumberUtils {
public:
    static bool isEven(int number) {
        return number % 2 == 0;
    }
};



The above code is considered good because it is:
Simple, one-liner solution.
Easy to read and understand.
Follows the KISS principle by avoiding overengineering.
3. YAGNI — You Aren't Gonna Need It
This principle states that "Always implement things when you actually need them, never when you just foresee that you need them.". In simple terms, don't add functionality until it's necessary. Avoid building features that you think you might need in the future. This principle helps to keep the codebase clean and reduces unnecessary complexity.

Example:
Assume you've been asked to build a note-taking app that allows users to Create a note and view notes.
Now, You start thinking ahead — “What if later they want categories? Or tagging? Or syncing with Google Drive? I should prepare for that!”. This creates a lot of unnecessary complexity and wastage of time.
Importance:
Reduced waste
Simplified codebase
Faster development
When NOT to use YAGNI:
When the requirements are well-known: If a feature is guaranteed and soon to be implemented, preparing for it now might be more efficient. For example:
You're writing a messaging service that currently supports only text, but your product team has committed to image support in 2 sprints.
Designing your data model to handle attachments now might save significant refactoring later.
Performance-Critical Areas: In systems where performance is a first-class concern, avoiding YAGNI might actually help — preemptively building and testing real-world usage patterns can catch bottlenecks early.
