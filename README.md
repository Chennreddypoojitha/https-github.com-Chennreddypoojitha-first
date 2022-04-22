public class Withdraw
{
  public static void main(String[] args) throws Exception
  {
    BufferedReader br=new BufferedReader(new InputStreamReader(System.in));
    int n=Integer.parseInt(br.readLine());
    if(n>15000)
    {
      System.out.println("ATM Cash Limit exceeds.");
    }
    else
    {
      if(n<500)
      {
        System.out.println(n/100+" Hundreds");
      }
      else
      {
        int h=5;
        int f=(n-500)/500;
        //System.out.println(n-500+" "+(n-500)/500+" "+(n-500)%500);
        h += ((n-500)%500)/100;
        if(h>5)
        {
          f=f+1;
          h=h-5;
        }
        System.out.println(f+" Five Hundreds and "+h+" Hundreds");
      }
    }
  }
}
java
Share
Follow
edited Sep 26, 2018 at 7:49
user avatar
mmvsbg
3,4581717 gold badges4848 silver badges6969 bronze badges
asked Sep 25, 2018 at 18:46
user avatar
Ibrahim S. Gerbiqi
14511 silver badge1010 bronze badges
4
Hi, welcome to StackOverflow. What is going wrong when you run the code you have provided? We need to know the expected and actual outputs, plus any error messages. Please edit the question accordingly. Thanks. – 
MandyShaw
 Sep 25, 2018 at 21:13
Add a comment
3 Answers
Sorted by:

Highest score (default)

3

The below code must do the task for you including money withdrawal in 500,100,50,20,10,1.

public class Withdraw
{
 public static void main(String[] args)
 {

      int moneyValue=2430;
      int[] noteValues= {500,100,50,20,10,1};
      if(moneyValue>15000)
      {
          System.out.println("ATM Cash Limit exceeds.");
      }
      else
      {
         for(int i=0;i<noteValues.length && moneyValue!=0;i++)
         {
             if(moneyValue>=noteValues[i])
                 System.out.println("No of "+noteValues[i]+"'s"+" :"+moneyValue/noteValues[i]);
             moneyValue=moneyValue%noteValues[i];
         }
      }
  }
}   



 Output:
  No of 500's :4
  No of 100's :4
  No of 20's :1
  No of 10's :1
Share
Follow
edited Sep 25, 2018 at 20:11
answered Sep 25, 2018 at 19:34
user avatar
rahul shetty
11177 bronze badges
Thank you this was what im looking for. – 
Ibrahim S. Gerbiqi
 Sep 25, 2018 at 21:15 
Add a comment

0

One way is to work backwards starting from the largest note:

//    n = 2430

int f = 0;
while (n >= 500) {
    f++;
    n -= 500;
}

int h= 0;
while (n >= 100) {
    h++;
    n -= 100;
}

// other values

System.out.println(f + " Five Hundreds and "+ h +" Hundreds" + ... );
Also, consider using well named variables. For example, instead of f, name it something like countOfFiveHundredEuroBankNotes.

Also, extract the constants, for example:

private static final int FIVE_HUNDRED_EUROS = 500;
which helps make the code more readable:

int remainingRequestedAmount = n;

while (remainingRequestedAmount >= FIVE_HUNDRED_EUROS) {
    countOfFiveHundredEuroBankNotes++;
    remainingRequestedAmount -= FIVE_HUNDRED_EUROS;
}
Share
Follow
edited Sep 25, 2018 at 19:12
answered Sep 25, 2018 at 19:01
user avatar
Andrew S
2,39411 gold badge99 silver badges1414 bronze badges
Add a comment

0

You could start with the largest bill amount and divide the money withdrawn. If that number is greater than 0, round it. That'll give you the quantity. You can then take away the quantity*billamount.

Than do the same to the next (as long as the amounts are in order from largest -> smallest)

E.G:

public class Withdraw
{
    public static void main(String[] args) throws Exception
    {
        int moneyWithdrawn = 2040;
        final int[] amountsFromLargestToSmallest = { 100, 50, 20, 10};
        final int smallestBillAmount = 10;

        int[] amountsOfEachBill = new int[amountsFromLargestToSmallest.length];

        if (moneyWithdrawn > 15000)
        {
            System.out.println("ATM Cash Limit exceeds.");
        }
        // Must be divisible by 10, since that's our smallest bill
        else if (moneyWithdrawn <= 0 || moneyWithdrawn % 10 != 0)
        {
            System.out.println("Please enter a valid number.");
        }
        else
        {
            for (int i = 0; i < amountsFromLargestToSmallest.length; i++)
            {
                int billAmount = amountsFromLargestToSmallest[i];

                int amount = (int) Math.floor(moneyWithdrawn / billAmount);

                if (amount > 0)
                {
                    moneyWithdrawn -= amount * billAmount;
                }

                amountsOfEachBill[i] = amount;
            }
        }
    }
}




https-github.com-Chennreddypoojitha-first
# 
