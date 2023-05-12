# banker's algorithm - George Hatem 19104121

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
    
    
    # here is my explaination to the code
    
    # "Resources," "available," "allocation," and "maximum" are the four arguments that the "banker" function accepts. These, in turn, stand for the total amount of resources, the number of resources that are now available, the number of resources that are currently allocated to processes, and the greatest requirement for each process.
    # The function initialises a few data structures, such as the 'work' vector (which depicts the quantity of resources currently available), the 'finish' vector (which tracks which processes have completed executing), and the 'need' matrix (which depicts the maximum quantity of each resource that each process needs to complete).
    # The code then moves into a loop that mimics how resources are distributed across processes. The function determines if a process may be safely performed given the state of the system throughout each iteration of the loop. If so, it allots the required resources to that process, updating the "work" and "finish" vectors as appropriate. The function returns 'None' in the absence of such a process, signifying that no safe allocation sequence is present.
    # The function enters another loop that enables the user to request more resources for a specific process if a secure sequence of allocations is discovered. The function uses the Banker's algorithm to determine if it is safe to give the requested resources. It checks to see if the resultant state is still secure and, if they can, provides the resources. If so, a notice stating that the request was approved is printed. If the outcome is hazardous, it publishes a message stating that the request was turned down because of the risky outcome. The function outputs a message stating that the request was rejected because the required resources could not be given instantly if they could not be granted safely.
    # The function keeps asking the user for requests until the user types 'q' to end the programme.
    # If there is a safe sequence of allocations detected, the method returns it.
