# banker's algorithm 

def banker(resources, available, allocation, maximum):
    # Step 1: Initialize data structures
    n = len(resources)
    work = available.copy()
    finish = [False] * n
    need = []
    for i in range(n):
        need.append([maximum[i][j] - allocation[i][j] for j in range(len(resources))])
    # Step 2: Find a safe sequence
    safe_seq = []
    while False in finish:
        found = False
        for i in range(n):
            if not finish[i] and all(need[i][j] <= work[j] for j in range(len(resources))):
                finish[i] = True
                for j in range(len(resources)):
                    work[j] += allocation[i][j]
                safe_seq.append(i)
                found = True
        if not found:
            return None  # No safe sequence found
    # Step 3: Check if the request can be granted
    def can_grant_request(request):
        for j in range(len(resources)):
            if request[j] > need[requesting_process][j] or request[j] > available[j]:
                return False
        return True
    def grant_request(request):
        for j in range(len(resources)):
            available[j] -= request[j]
            allocation[requesting_process][j] += request[j]
            need[requesting_process][j] -= request[j]
    def undo_grant_request(request):
        for j in range(len(resources)):
            available[j] += request[j]
            allocation[requesting_process][j] -= request[j]
            need[requesting_process][j] += request[j]
    def is_safe_state():
        work = available.copy()
        finish = [False] * n
        while False in finish:
            found = False
            for i in range(n):
                if not finish[i] and all(need[i][j] <= work[j] for j in range(len(resources))):
                    finish[i] = True
                    for j in range(len(resources)):
                        work[j] += allocation[i][j]
                    found = True
            if not found:
                return False
        return True
    while True:
        print("\nCurrent state:")
        print("Resources:", resources)
        print("Available:", available)
        print("Allocation:", allocation)
        print("Maximum:", maximum)
        action = input("Enter 'r' for resource request, 'q' to quit: ")
        if action == 'q':
            break
        if action == 'r':
            requesting_process = int(input("Enter requesting process number (0-indexed): "))
            request = [int(x) for x in input("Enter request vector: ").split()]
            if can_grant_request(request):
                grant_request(request)
                if is_safe_state():
                    print("Request granted")
                else:
                    print("Request denied - unsafe state")
                    undo_grant_request(request)
            else:
                print("Request denied - cannot be granted immediately")
    return safe_seq
