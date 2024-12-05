#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <math.h>

void ipToArray(const char *ip, int arr[4]) {
    sscanf(ip, "%d.%d.%d.%d", &arr[0], &arr[1], &arr[2], &arr[3]);
}
void calculateNetworkAddress(int ip[4], int mask[4], int network[4]) {
    for (int i = 0; i < 4; i++) {
        network[i] = ip[i] & mask[i];
    }
}
void calculateBroadcastAddress(int ip[4], int mask[4], int broadcast[4]) {
    for (int i = 0; i < 4; i++) {
        broadcast[i] = ip[i] | (~mask[i] & 255);
    }
}
void printIP(const char *label, int ip[4]) {
    printf("%s: %d.%d.%d.%d\n", label, ip[0], ip[1], ip[2], ip[3]);
}

int main() {
    char ipStr[16], maskStr[16];
    int ip[4], mask[4], network[4], broadcast[4];
    printf("Enter IP address (e.g., 192.168.1.10): ");

    scanf("%15s", ipStr);
    printf("Enter Subnet Mask (e.g., 255.255.255.0): ");
    scanf("%15s", maskStr);

    ipToArray(ipStr, ip);
    ipToArray(maskStr, mask);
    calculateNetworkAddress(ip, mask, network);
    calculateBroadcastAddress(ip, mask, broadcast);

    printIP("IP Address", ip);
    printIP("Subnet Mask", mask);
    printIP("Network Address", network);
    printIP("Broadcast Address", broadcast);

    int firstUsable[4], lastUsable[4];
    for (int i = 0; i < 4; i++) {
        firstUsable[i] = network[i];
        lastUsable[i] = broadcast[i];
    }
    firstUsable[3] += 1; // First usable IP
    lastUsable[3] -= 1;  // Last usable IP

    printIP("First Usable IP", firstUsable);
    printIP("Last Usable IP", lastUsable);

    return 0;
}
