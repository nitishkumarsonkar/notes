# Systematic Problem-Solving Framework

## Core Framework

1. **Clearly define the problem**
   - Identify what you're trying to solve precisely
   - Determine the expected output/result
   - Establish constraints and requirements

2. **Break down the problem**
   - Divide complex problems into smaller, manageable parts
   - Identify the core components that need solutions

3. **Analyze patterns and connections**
   - Look for similarities to problems you've solved before
   - Identify patterns that might suggest solution approaches

4. **Generate multiple solution strategies**
   - Brainstorm different approaches without immediately judging them
   - Consider both conventional and creative solutions

5. **Evaluate and select an approach**
   - Assess each potential solution against requirements
   - Consider efficiency, simplicity, and maintainability
   - Choose the most promising approach

6. **Implement systematically**
   - Work through your solution step by step
   - Start with the simplest components first

7. **Test and verify**
   - Validate against expected outcomes
   - Test edge cases and boundary conditions

8. **Refine and optimize**
   - Look for improvements in efficiency or clarity
   - Simplify where possible

## Example 1: Finding Duplicate Elements in an Array

### 1. Define the problem
- Input: Array of integers
- Output: List of elements that appear more than once
- Constraints: Optimize for time/space efficiency

### 2. Break down the problem
- We need to count occurrences of each element
- Then identify which elements have multiple occurrences

### 3. Analyze patterns
- This is a counting/frequency problem
- Similar to other problems requiring element tracking

### 4. Generate strategies
- Nested loops approach (simple but inefficient)
- Sorting approach (efficient for certain scenarios)
- Hash-based approach (generally efficient)

### 5. Evaluate and select
- The hash-based approach offers O(n) time complexity with reasonable space requirements

### 6. Implement systematically
```java
public List<Integer> findDuplicates(int[] nums) {
    Map<Integer, Integer> countMap = new HashMap<>();
    List<Integer> result = new ArrayList<>();
    
    // Count occurrences
    for (int num : nums) {
        countMap.put(num, countMap.getOrDefault(num, 0) + 1);
    }
    
    // Find elements with count > 1
    for (Map.Entry<Integer, Integer> entry : countMap.entrySet()) {
        if (entry.getValue() > 1) {
            result.add(entry.getKey());
        }
    }
    
    return result;
}
```

### 7. Test and verify
- Test with array containing duplicates: [1, 2, 3, 1, 4, 2] → [1, 2]
- Test with no duplicates: [1, 2, 3, 4] → []
- Test with all duplicates: [1, 1, 1] → [1]

### 8. Refine
- Consider if we can optimize further (possibly using a Set for specific cases)
- Assess if the solution handles all edge cases properly

## Example 2: Implementing Dijkstra's Algorithm

### 1. Define the Problem
- Input: Graph with weighted edges, start node, end node
- Output: Shortest path from start to end and its total distance
- Constraints: Weights are positive, graph may be large and dense

### 2. Break Down the Problem
- We need to track distances from start to every node
- We need to track the path taken (which nodes we visited)
- We need an efficient way to always process the "closest" unprocessed node next

### 3. Analyze Patterns
- This is a classic shortest path problem
- We need to explore nodes in order of increasing distance from the start
- We need to relax edges when we find shorter paths

### 4. Generate Solution Strategies
- Brute force approach (try all possible paths)
- Dynamic programming approach
- Greedy approach with priority queue (Dijkstra's algorithm)
- A* algorithm if we have heuristics about the graph

### 5. Evaluate and Select
- Dijkstra's with a priority queue offers O(E + V log V) time complexity, which is efficient for this problem
- It's suitable for most graph types without negative edges

### 6. Implement Systematically

```java
public class DijkstraShortestPath {
    static class Edge {
        int destination;
        int weight;
        
        Edge(int destination, int weight) {
            this.destination = destination;
            this.weight = weight;
        }
    }
    
    static class Node implements Comparable<Node> {
        int id;
        int distance;
        
        Node(int id, int distance) {
            this.id = id;
            this.distance = distance;
        }
        
        @Override
        public int compareTo(Node other) {
            return Integer.compare(this.distance, other.distance);
        }
    }
    
    public static List<Integer> findShortestPath(List<List<Edge>> graph, int start, int end) {
        int n = graph.size();
        int[] distances = new int[n];
        int[] predecessors = new int[n];
        boolean[] visited = new boolean[n];
        
        // Initialize distances
        Arrays.fill(distances, Integer.MAX_VALUE);
        Arrays.fill(predecessors, -1);
        distances[start] = 0;
        
        // Priority queue to process nodes by distance
        PriorityQueue<Node> queue = new PriorityQueue<>();
        queue.add(new Node(start, 0));
        
        while (!queue.isEmpty()) {
            Node current = queue.poll();
            int nodeId = current.id;
            
            // Skip if we've already processed this node
            if (visited[nodeId]) continue;
            
            // Mark as visited
            visited[nodeId] = true;
            
            // If we reached our destination, we're done
            if (nodeId == end) break;
            
            // Explore all neighbors
            for (Edge edge : graph.get(nodeId)) {
                int neighbor = edge.destination;
                
                if (!visited[neighbor]) {
                    int newDistance = distances[nodeId] + edge.weight;
                    
                    // If we found a shorter path
                    if (newDistance < distances[neighbor]) {
                        distances[neighbor] = newDistance;
                        predecessors[neighbor] = nodeId;
                        queue.add(new Node(neighbor, newDistance));
                    }
                }
            }
        }
        
        // Reconstruct the path
        if (distances[end] == Integer.MAX_VALUE) {
            return Collections.emptyList(); // No path exists
        }
        
        List<Integer> path = new ArrayList<>();
        for (int at = end; at != -1; at = predecessors[at]) {
            path.add(at);
        }
        Collections.reverse(path);
        
        return path;
    }
    
    // Method to build the graph (adjacency list)
    public static List<List<Edge>> buildGraph(int n, int[][] edges) {
        List<List<Edge>> graph = new ArrayList<>();
        
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }
        
        for (int[] edge : edges) {
            int from = edge[0];
            int to = edge[1];
            int weight = edge[2];
            
            graph.get(from).add(new Edge(to, weight));
            // For undirected graph, add this line:
            // graph.get(to).add(new Edge(from, weight));
        }
        
        return graph;
    }
    
    // Example usage
    public static void main(String[] args) {
        int n = 6; // Number of nodes
        int[][] edgesArray = {
            {0, 1, 4}, {0, 2, 2}, {1, 2, 5}, {1, 3, 10},
            {2, 3, 3}, {2, 4, 8}, {3, 5, 7}, {4, 5, 1}
        };
        
        List<List<Edge>> graph = buildGraph(n, edgesArray);
        List<Integer> shortestPath = findShortestPath(graph, 0, 5);
        
        System.out.println("Shortest path: " + shortestPath);
    }
}
```

### 7. Test and Verify
- Test with small graph examples with known shortest paths
- Test edge cases like:
  - Disconnected graphs (no path exists)
  - Direct edge between start and end
  - Multiple possible paths with same total distance
  - Large graphs with many nodes and edges

### 8. Refine and Optimize
- Consider space optimization by using more compact data structures
- Implement early termination when we reach the target node
- Add functionality to calculate and return the total distance
- Explore bidirectional search for even better performance

## Example 3: Customer Service Process Optimization

### 1. Define the Problem
- **Issue**: Long customer wait times leading to customer dissatisfaction
- **Goal**: Reduce average wait time by 50% while maintaining or improving service quality
- **Key metrics**: Average wait time, customer satisfaction score, resolution time, first-call resolution rate

### 2. Break Down the Problem
- Call volume patterns throughout day/week
- Staff scheduling efficiency
- Call routing system effectiveness
- Customer service representative training and expertise
- Common issues that generate the most calls
- Current technology infrastructure limitations

### 3. Analyze Patterns
- Identify peak call times vs. staffing levels
- Analyze types of inquiries that take longest to resolve
- Review customer feedback for recurring themes
- Examine call abandonment rates and correlations
- Study high-performing representatives' techniques

### 4. Generate Solution Strategies
- Implement intelligent call routing based on issue type
- Revise staff scheduling to match call volume patterns
- Develop self-service options for common inquiries
- Create specialized teams for complex issues
- Improve training program for representatives
- Introduce callback options during peak times
- Develop better knowledge base for representatives

### 5. Evaluate and Select
- Based on data analysis, prioritize strategies with:
  - Highest impact on wait time reduction
  - Reasonable implementation cost and timeframe
  - Minimal disruption to existing operations
  - Sustainable long-term benefits
- Select top 3-4 initiatives for implementation

### 6. Implement Systematically
- **Phase 1**: Optimized staff scheduling aligned with call volume data
  - Create new schedules based on historical call patterns
  - Implement flexible scheduling for peak times
  
- **Phase 2**: Enhanced self-service portal
  - Identify top 10 most common customer inquiries
  - Develop clear, step-by-step solutions accessible online
  - Create video tutorials for complex procedures
  
- **Phase 3**: Improved call routing system
  - Categorize calls by complexity and urgency
  - Direct specialized inquiries to subject matter experts
  - Implement customer history recognition for personalized service

- **Phase 4**: Training program enhancement
  - Develop knowledge sharing sessions between high and low performers
  - Create standardized resolution paths for common issues
  - Build comprehensive knowledge base accessible during calls

### 7. Test and Verify
- Run a pilot program with selected strategies for 2 weeks
- Collect data on key metrics:
  - Average wait time (before and after implementation)
  - Customer satisfaction scores
  - Representative feedback
  - Call resolution times
  - First-call resolution rate
- Compare results against baseline and targets

### 8. Refine and Optimize
- Analyze pilot results to identify:
  - Which strategies produced greatest improvements
  - Unexpected challenges or bottlenecks
  - Areas requiring further attention
- Make adjustments based on findings:
  - Scale successful strategies
  - Modify or abandon underperforming initiatives
  - Address newly identified issues
- Implement continuous improvement process with regular review cycles

## Key Principles for Effective Problem Solving

1. **Define success criteria before starting** - Know what "solved" looks like
2. **Focus on root causes, not symptoms** - Dig deeper to find underlying issues
3. **Keep solutions as simple as possible** - Avoid unnecessary complexity
4. **Test assumptions rigorously** - Validate your understanding with data
5. **Learn from failures** - Use unsuccessful attempts as learning opportunities
6. **Document your process** - Create knowledge for future problem solving
7. **Maintain flexibility** - Be willing to pivot when evidence suggests a better path
8. **Seek diverse perspectives** - Different viewpoints reveal blind spots
