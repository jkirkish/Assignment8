# Assignment8
package com.coderscampus.Assignment8;



import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.Map;
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Assignment8Application {
	public static void main(String[] args) {
		Assignment8 assignment8 = new Assignment8();
		
		List<Integer> allNumbers = Collections.synchronizedList(new ArrayList<>(1000));
		
		
		
		
		ExecutorService executor = Executors.newCachedThreadPool();
		
		List<CompletableFuture<Void>> tasks = new ArrayList<>(1000);
		
		for (int i=0; i<1000; i++) {
			CompletableFuture<Void> task = 
				CompletableFuture.supplyAsync(() -> assignment8.getNumbers(), executor)
								 .thenAccept(numbers -> allNumbers.addAll(numbers));
			tasks.add(task);
		}
		
		while (tasks.stream().filter(CompletableFuture::isDone).count() < 1000 ) {
			try {
				Thread.sleep(100);
			} catch (InterruptedException e) {
			}
		}
		
		showEachNumber(allNumbers);
       
		// This is the "For Loop" way of doing things
//		Map<Integer, Integer> output = new HashMap<>();
//		allNumbers.stream()
//		          .forEach(number -> {
//		        	  if (!output.containsKey(number)) {
//		        		  output.put(number, 1);
//		        	  } else {
//		        		  output.put(number, output.get(number) + 1);
//		        	  }
//		          });
//		System.out.println(output);
		
		// this is the Stream Collector.toMap way of doing things
		Map<Integer, Integer> output = allNumbers.stream()
				  .collect(Collectors.toMap(i -> i, i -> 1, (oldValue, newValue) -> oldValue + 1));
		System.out.println(output);
		runExecutorStats(executor);
	}
	
	public static void runExecutorStats(ExecutorService executor) {
		
		ThreadPoolExecutor pool = (ThreadPoolExecutor) executor;
		
		System.out.println("Core threads: " + pool.getCorePoolSize());
	      System.out.println("Largest executions: "
	         + pool.getLargestPoolSize());
	      System.out.println("Maximum allowed threads: "
	         + pool.getMaximumPoolSize());
	      System.out.println("Current threads in pool: "
	         + pool.getPoolSize());
	      System.out.println("Currently executing threads: "
	         + pool.getActiveCount());
	      System.out.println("Total number of threads(ever scheduled): "
	         + pool.getTaskCount());

	      executor.shutdown();
	}
	public static void showEachNumber(List<Integer> aList) {
	          aList.parallelStream()
	               .anyMatch(match -> switch(match){
	                   int zero,one, two, three, four, five, six,seven, eight,nine,ten,elev,twelv,thirt,fourt;
	                     case 0: zero++;
	                            break;
	                     case 1: one++;
                         		break;
	                     case 2: two++;
                        		break;
	                     case 3: three++;
	                     		break; 
	                     case 4: four++;
                         		break;
	                     case 5: five++;
	                     		break;
	                     case 6: six++;
	                     		break;
	                     case 7: seven++;
	                     		break;  
	                     case 8: eight++;
                         		break;
	                     case 9: nine++;
                   				break;
	                     case 10: ten++;
                  				break;
	                     case 11: eleven++;
                   				break; 
	                     case 12: twelv++;
                   				break;
	                     case 13: thirt++;
                   				break;
	                     case 14: fourt++;
                   				break;
	                    
	               } );
		     
		
	}
	
}
