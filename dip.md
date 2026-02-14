
Introduction
There is a set of five principles for writing clean, scalable, maintainable object-oriented code. These principles are known as SOLID principles. The D in SOLID stands for Dependency Inversion Principle.

Definition:
"High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions."

In simpler words, Rather than high-level classes controlling and depending on the details of lower-level ones, both should rely on interfaces or abstract classes. This makes your code flexible, testable, and easier to maintain.
Real-life Analogy
Let's say you're hungry and you want pizza. You use a food delivery app, and not contact the chef directly.

You (user) → Use → Food App (Abstraction)
Food App → Deals with → Restaurant/Chef (Implementation)

Understanding
You don't care which chef will make the pizza or how the pizza is made or who is your delivery partner — you just want it delivered from your selected restaurant on time.

Here:
You = High-level module
Food App Interface = Abstraction
Restaurant = Low-level module

You're not directly dependent on any specific details, but only on the food delivery system (abstraction).

This is what exactly DIP suggests while writing code with industry standards — high-level modules should not depend on low-level modules. Instead, both should depend on abstractions.
Example: Netflix Recommendation Engine
Let's illustrate the Dependency Inversion Principle with a simple example of a Netflix recommendation engine.

Netflix uses various recommendation strategies:

Recently Added: Shows/movies recently added to the catalog
Trending Now: Based on what's currently popular
Genre-Based: What you've watched and liked before

Now let's see how Netflix might (badly) and should (correctly) implements this using the Dependency Inversion Principle.
Without DIP — Tightly Coupled Code

C++


#include <iostream>
using namespace std;

// Class implementing the recommendations based on recently added
class RecentlyAdded {
public:
    // Method to get the recommendations
    void getRecommendations() {
        cout << "Showing recently added content..." << endl;
    }
};

// Class implementing the overall Recommendation Engine
class RecommendationEngine {
private:
    RecentlyAdded recommender;

public:
    void recommend() {
        recommender.getRecommendations();
    }
};

Issues in the above code:
RecommendationEngine is tightly coupled to RecentlyAdded.
If we want to switch to TrendingNow or GenreBased strategies, we have to modify the engine.
With DIP — Using Abstraction

C++


#include <iostream>
using namespace std;

// Interface provided for classes to implement different recommendation strategies
class RecommendationStrategy {
public:
    virtual void getRecommendations() = 0;
    virtual ~RecommendationStrategy() {}
};

// Class implementing recommendations based on recently added
class RecentlyAdded : public RecommendationStrategy {
public:
    void getRecommendations() override {
        cout << "Showing recently added content..." << endl;
    }
};

// Class implementing recommendations based on trending now
class TrendingNow : public RecommendationStrategy {
public:
    void getRecommendations() override {
        cout << "Showing trending content..." << endl;
    }
};

// Class implementing recommendations based on Genre
class GenreBased : public RecommendationStrategy {
public:
    void getRecommendations() override {
        cout << "Showing content based on your favorite genres..." << endl;
    }
};

// Class implementing the Recommendation Engine (High - level module)
class RecommendationEngine {
private:
    RecommendationStrategy* strategy;

public:
    RecommendationEngine(RecommendationStrategy* strategy) : strategy(strategy) {}

    void recommend() {
        strategy->getRecommendations();
    }
};

// Main driver code
int main() {
    RecommendationStrategy* strategy = new TrendingNow(); // could also be RecentlyAdded or GenreBased
    RecommendationEngine engine(strategy);
    engine.recommend();

    delete strategy;
    return 0;
}

Here:
RecommendationEngine doesn't care how recommendations are made — it just needs a recommendation.
The strategies (TrendingNow, RecentlyAdded, GenreBased) can be switched or upgraded anytime, without changing the engine.

Easier Switching between Strategies at Runtime
Let's say a user switches from "Recently Added" to "Genre-Based" dynamically:

C++


#include <iostream>
using namespace std;

// Interface provided for classes to implement different recommendation strategies
class RecommendationStrategy {
public:
    virtual void getRecommendations() = 0;
    virtual ~RecommendationStrategy() {}
};

// Class implementing recommendations based on recently added
class RecentlyAdded : public RecommendationStrategy {
public:
    void getRecommendations() override {
        cout << "Showing recently added content..." << endl;
    }
};

// Class implementing recommendations based on trending now
class TrendingNow : public RecommendationStrategy {
public:
    void getRecommendations() override {
        cout << "Showing trending content..." << endl;
    }
};

// Class implementing recommendations based on Genre
class GenreBased : public RecommendationStrategy {
public:
    void getRecommendations() override {
        cout << "Showing content based on your favorite genres..." << endl;
    }
};

// Class implementing the Recommendation Engine (High - level module)
class RecommendationEngine {
private:
    RecommendationStrategy* strategy;

public:
    RecommendationEngine(RecommendationStrategy* strategy) : strategy(strategy) {}

    void recommend() {
        strategy->getRecommendations();
    }
};

// Main driver code
int main() {
    RecommendationEngine engine(new GenreBased());
    engine.recommend();
    return 0;
}

No changes required in RecommendationEngine class — just pass a new strategy. That's the power of Dependency Inversion Principle used in designing the Recommendation Strategy.
Benefits of using DIP
There are various benefits to using the Dependency Inversion Principle (DIP) in software design. Here are some of the key advantages:

Flexibility: Easily swap out implementations without modifying high-level code.
Testability: You can mock or stub the abstractions during testing.
Reusability: Code becomes reusable since it's not tightly bound to one specific implementation.
Maintainability: Makes it easier to change one part of the system without affecting others.
Scalability: You can scale or upgrade parts of your codebase without a massive rewrite
