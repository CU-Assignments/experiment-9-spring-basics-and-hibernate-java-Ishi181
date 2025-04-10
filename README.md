[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/9RndFDpw)

# Max Units on a Truck	

class Solution {
public:
    int maximumUnits(vector<vector<int>>& boxTypes, int truckSize) {
        // Sort the boxTypes in descending order based on the number of units per box
        sort(boxTypes.begin(), boxTypes.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[1] > b[1];
        });

        int maxUnits = 0;

        for (const auto& box : boxTypes) {
            int boxCount = min(truckSize, box[0]); // Take as many boxes as possible
            maxUnits += boxCount * box[1];
            truckSize -= boxCount; // Reduce the truck size
            
            if (truckSize == 0) {
                break; // Stop if the truck is full
            }
        }

        return maxUnits;
    }
};


# OUTPUT

# Min Operations to make array incresing	

class Solution {
public:
    int minOperations(vector<int>& nums) {
        int operations = 0;

        for (int i = 1; i < nums.size(); i++) {
            if (nums[i] <= nums[i - 1]) {
                operations += (nums[i - 1] - nums[i] + 1);
                nums[i] = nums[i - 1] + 1; // Increment to maintain strictly increasing order
            }
        }

        return operations;
    }
};


# OUTPUT

# Remove stones to Maximize total	CW	

#include <iostream>
#include <vector>
#include <queue>
using namespace std;

class Solution {
public:
    int minStoneSum(vector<int>& piles, int k) {
        priority_queue<int> maxHeap(piles.begin(), piles.end());

        while (k-- > 0) {
            int maxPile = maxHeap.top();
            maxHeap.pop();
            int stonesRemoved = maxPile / 2;
            maxHeap.push(maxPile - stonesRemoved);
        }

        int totalStones = 0;
        while (!maxHeap.empty()) {
            totalStones += maxHeap.top();
            maxHeap.pop();
        }

        return totalStones;
    }
};


# OUTPUT
# Max Score from removing substrings	

class Solution {
public:
    int maximumGain(string s, int x, int y) {
        if (y > x) {
            swap(x, y);
            reverse(s.begin(), s.end());
        }
        
        int points = 0;
        stack<char> st;

        // First remove "ab" if x >= y
        for (char ch : s) {
            if (!st.empty() && st.top() == 'a' && ch == 'b') {
                st.pop();
                points += x;
            } else {
                st.push(ch);
            }
        }

        // Remaining string after "ab" removals
        string remaining;
        while (!st.empty()) {
            remaining += st.top();
            st.pop();
        }
        reverse(remaining.begin(), remaining.end());

        // Now remove "ba"
        for (char ch : remaining) {
            if (!st.empty() && st.top() == 'b' && ch == 'a') {
                st.pop();
                points += y;
            } else {
                st.push(ch);
            }
        }

        return points;
    }
};


# OUTPUT
# Min operations to make a subsequence	

#include <vector>
#include <unordered_map>
#include <algorithm>
using namespace std;

class Solution {
public:
    int minOperations(vector<int>& target, vector<int>& arr) {
        unordered_map<int, int> indexMap;
        
        // Map target values to their indices
        for (int i = 0; i < target.size(); i++) {
            indexMap[target[i]] = i;
        }
        
        vector<int> indexList;
        
        // Create the index list from arr using the indexMap
        for (int num : arr) {
            if (indexMap.find(num) != indexMap.end()) {
                indexList.push_back(indexMap[num]);
            }
        }
        
        // Find the Longest Increasing Subsequence (LIS) using binary search
        vector<int> lis;
        for (int index : indexList) {
            auto it = lower_bound(lis.begin(), lis.end(), index);
            if (it == lis.end()) {
                lis.push_back(index);
            } else {
                *it = index;
            }
        }
        
        // Calculate the result
        return target.size() - lis.size();
    }
};


# OUTPUT
# Max number of tasks you can assign	CW

#include <vector>
#include <algorithm>
#include <set>
using namespace std;

class Solution {
public:
    bool canAssign(int mid, vector<int>& tasks, vector<int>& workers, int pills, int strength) {
        multiset<int> workerSet(workers.begin(), workers.end());
        int remainingPills = pills;

        for (int i = mid - 1; i >= 0; --i) {
            auto it = workerSet.lower_bound(tasks[i]);
            if (it != workerSet.end()) {
                workerSet.erase(it);
            } else {
                if (remainingPills == 0) return false;
                it = workerSet.lower_bound(tasks[i] - strength);
                if (it == workerSet.end()) return false;
                workerSet.erase(it);
                remainingPills--;
            }
        }
        return true;
    }

    int maxTaskAssign(vector<int>& tasks, vector<int>& workers, int pills, int strength) {
        sort(tasks.begin(), tasks.end());
        sort(workers.begin(), workers.end());
        
        int low = 0, high = min(tasks.size(), workers.size());
        int result = 0;

        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (canAssign(mid, tasks, workers, pills, strength)) {
                result = mid;
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return result;
    }
};


# OUTPUT
