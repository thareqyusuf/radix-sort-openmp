// C++ implementation of Radix Sort
#include <stdio.h>
#include "mpi.h"

// A utility function to get maximum value in arr[]
int getMax(int arr[], int n)
{
	// RANK
	int rank;
	MPI_Comm_rank(MPI_COMM_WORLD, &rank);
	// NUMBER OF PROCESS
	int numProc;
	MPI_Comm_size(MPI_COMM_WORLD, &numProc);
	// Find Local Max
	int num_elements_per_proc = n / numProc;
	int local_max = 0;
	int i;
	for (i = rank * num_elements_per_proc; i < (rank + 1) * num_elements_per_proc; i++)
	{
		if (arr[i] > local_max)
		{
			local_max = arr[i];
		}
	}
	printf("LOCAL MAX PROC %d : %d\n", rank, local_max);
	// Reduce all of the local max
	int global_max;
	MPI_Allreduce(&local_max, &global_max, 1, MPI_INT, MPI_MAX, MPI_COMM_WORLD);
	printf("Global MAX PROC %d : %d\n", rank, global_max);
	return global_max;

	// int mx = arr[0];
	// for (int i = 1; i < n; i++)
	// 	if (arr[i] > mx)
	// 		mx = arr[i];
	// return mx;
}

// A function to do counting sort of arr[] according to
// the digit represented by exp.
void countSort(int arr[], int n, int exp)
{
	// RANK
	int rank;
	MPI_Comm_rank(MPI_COMM_WORLD, &rank);
	// NUMBER OF PROCESS
	int numProc;
	MPI_Comm_size(MPI_COMM_WORLD, &numProc);
	printf("EXP %d PROC %d\n", exp, rank);
	int num_elements_per_proc = n / numProc;
	int output[n]; // output array
	int i;
	int count_local[10] = {0};
	// COUNT LOCAL
	for (i = rank * num_elements_per_proc; i < (rank + 1) * num_elements_per_proc; i++)
	{
		count_local[(arr[i] / exp) % 10]++;
	}
	printf("Count Local Proc %d :\n", rank);
	for (int i = 0; i < 10; i++)
	{
		printf("%d : %d, ", i, count_local[i]);
	}
	printf("\n");
	// REDUCE ALL OF THE LOCAL COUNT
	int count[10] = {0};
	MPI_Allreduce(&count_local, &count, 10, MPI_INT, MPI_SUM, MPI_COMM_WORLD);
	// PRINT ALL COUNT
	printf("Global COUNT PROC %d :\n", rank);
	for (int i = 0; i < 10; i++)
	{
		printf("%d : %d, ", i, count[i]);
	}
	printf("\n");
	// // Store count of occurrences in count[]
	// for (i = 0; i < n; i++)
	// 	count[(arr[i] / exp) % 10]++;

	if (rank == 0)
	{
		// Change count[i] so that count[i] now contains actual
		// position of this digit in output[]
		for (i = 1; i < 10; i++)
			count[i] += count[i - 1]; //Ga bisa diparalelkan karena ada i - 1

		// Build the output array
		for (i = n - 1; i >= 0; i--)
		{
			output[count[(arr[i] / exp) % 10] - 1] = arr[i];
			count[(arr[i] / exp) % 10]--;
		}

		// Copy the output array to arr[], so that arr[] now
		// contains sorted numbers according to current digit
		for (i = 0; i < n; i++)
			arr[i] = output[i];
		print(arr, n);
	}
	MPI_Bcast(arr, n, MPI_INT, 0, MPI_COMM_WORLD);
}

// The main function to that sorts arr[] of size n using
// Radix Sort
void radixsort(int arr[], int n)
{
	int rank;
	MPI_Comm_rank(MPI_COMM_WORLD, &rank);
	// Find the maximum number to know number of digits
	int m = getMax(arr, n);

	// Do counting sort for every digit. Note that instead
	// of passing digit number, exp is passed. exp is 10^i
	// where i is current digit number
	// if(rank == 0){
	for (int exp = 1; m / exp > 0; exp *= 10)
	{
		countSort(arr, n, exp);
	}
	// }else{
	// 	printf("rank %d stop\n", rank);
	// 	return;
	// }
}

// A utility function to print an array
void print(int arr[], int n)
{
	for (int i = 0; i < n; i++)
		printf("%d ", arr[i]);
	printf("\n");
}

// Driver program to test above functions
int main(int argc, char *argv[])
{
	int arr[] = {170, 45, 75, 90, 802, 24, 2, 66};
	// 170, 090, 802, 002, 024, 045, 075, 066
	// 802, 002, 024, 045, 066, 170, 075, 090
	// 002, 024, 045, 066, 075, 090, 170, 802
	int n = sizeof(arr) / sizeof(arr[0]);
	MPI_Init(&argc, &argv);
	int rank;
	MPI_Comm_rank(MPI_COMM_WORLD, &rank);
	radixsort(arr, n);
	if (rank == 0)
	{
		print(arr, n);
	}
	MPI_Finalize();
	return 0;
}
