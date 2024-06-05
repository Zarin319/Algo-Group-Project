# Algo-Group-Project
Share Your Code Here

# Problem_1_code
class Vertex:
    def __init__(self, key):
        self.id = key
        self.connectedTo = []
        self.color = 'WHITE'
        self.parent = None
        self.discovery_time = 0
        self.finishing_time = 0

class Graph:
    def __init__(self):
        self.vertList = {}

    def addVertex(self, key):
        newVertex = Vertex(key)
        self.vertList[key] = newVertex

    def getVertex(self, n):
        if n in self.vertList:
            return self.vertList[n]
        else:
            return None

    def add_edge(self, f, t):
        if f not in self.vertList:
            self.addVertex(f)
        if t not in self.vertList:
            self.addVertex(t)
        self.vertList[f].connectedTo.append(self.vertList[t])

    def getVertices(self):
        return self.vertList.keys()

    def __iter__(self):
        return iter(self.vertList.values())

# Global time variable for tracking discovery and finishing times
time = 0

def DFS(graph, start_vertex, method):
    global time
    time = 0
    result = []

    if method == "recursive":
        def DFS_VISIT_RECURSIVE(u):
            global time
            time += 1
            u.discovery_time = time
            u.color = 'GRAY'
            result.append(u.id)
            for v in u.connectedTo:
                if v.color == 'WHITE':
                    v.parent = u
                    DFS_VISIT_RECURSIVE(v)
            u.color = 'BLACK'
            time += 1
            u.finishing_time = time

        for vertex in graph:
            if vertex.color == 'WHITE':
                DFS_VISIT_RECURSIVE(vertex)

    elif method == "iterative":
        stack = [start_vertex]
        while stack:
            vertex = stack.pop()
            if vertex.color == 'WHITE':
                time += 1
                vertex.discovery_time = time
                vertex.color = 'GRAY'
                result.append(vertex.id)
                for neighbor in reversed(vertex.connectedTo):
                    if neighbor.color == 'WHITE':
                        stack.append(neighbor)
                        neighbor.parent = vertex
                vertex.color = 'BLACK'
                time += 1
                vertex.finishing_time = time

    return result

# Example usage
g = Graph()
edges = [(1, 2), (1, 6), (2, 3), (3, 8), (8, 7), (7, 12), (12, 11), (11, 6), (12, 17), (17, 16), (16, 21), (17, 22),
         (8, 9), (9, 10), (10, 5), (5, 4), (10, 15), (15, 20), (20, 19), (19, 14), (8, 13), (13, 18), (18, 23), (23, 24),
         (24, 25)]

for f, t in edges:
    g.add_edge(f, t)

method = input("Choose DFS method - Type 'recursive' or 'iterative': ")
start_vertex = g.getVertex(1)  # Assuming you want to start from vertex 1
result = DFS(g, start_vertex, method)
print(f"Following is Depth First Traversal ({method}, starting from vertex 1):")
print(" ".join(map(str, result)))

# Output timestamps
for v in g:
    print(f"Vertex {v.id}: Discovery Time = {v.discovery_time}, Finishing Time = {v.finishing_time}")

    
