Карманная сортировка
========================================

Если входные элементы подчиняются равномерному распределению, то время выполнения алгоритма карманной сортировки линейно зависит от количества входных данных. Если при сортировки методом подсчета предполагается, что входные данные целочисленны и положительны, то при карманной сортировке предполагается, что они равномерно распределены в интервале от [0, 1)/

Идея, данного алгоритма заключается в том, чтобы разбить интервал от [0, 1) на n одинаковых интервалов, или карманов (buckets), а затем равномерно распределить по этим карманам n входных величин. Поскольку входные числа равномерно распределены в интервале [0,1), мы предполагаем, что в каждый из карманов попадет не много элементов. Чтобы получить выходную последовательность, нужно просто выполнить сортировку чисел в каждом кармане, а затем последовательно перечислить элементы каждого кармана.

Реализация алгоритма на Java
----------------------------

::

	private double arr[];
	private LinkedList<Double> buckets[];
	
	public BucketSort(double array[]){
		arr = array;
	}

	public LinkedList<Double> sort(){
		int length = arr.length;
		buckets = new LinkedList[arr.length];
		
		for (int j = 0; j < buckets.length; j++){
			buckets[j] = new LinkedList<Double>();
		}
		
		for (int i = 0; i < arr.length; i++){
			int bucketIndex = (int) Math.floor((arr[i] * length));
			buckets[bucketIndex].add(arr[i]);
		}
		
		// сортируем каждый список
		for (int j = 0; j < buckets.length; j++){
			sortBucketElements(buckets[j]);
		}
		
		// объединяем элементы списков
		return unionBuckets(buckets);
	}

	private void sortBucketElements(LinkedList<Double> list){
		
		for (int j = 1; j < list.size(); j++){
			double key = list.get(j);
			int i = j - 1;
			
			while ((i > -1) && (list.get(i) > key)){
				list.set(i + 1, list.get(i));
				--i;
			}
			
			list.set(i + 1, key);
		}
	}

	private LinkedList<Double> unionBuckets(LinkedList<Double> buckets[]){
		LinkedList<Double> outputList = new LinkedList<Double>();
		
		for (int i = 0; i < buckets.length; i++){
			outputList.addAll(buckets[i]);
		}
		
		return outputList;
	}


Анализ времени выполнения алгоритма
-----------------------------------
	
