# scala

## 基本语法
for(i <- 1 to 3; j <-1 to 4 if i != j) {printf("i=%d, j=%d, res=%d",i,j,i*j);println()}


for(x <- 1 to 10) yield x %2;


def add(a:Int,b:Int):Int = {var c = a+b;c;}

def decorate(prefix:String,str:String,suffix:String) = {prefix + str + suffix}

def add(a:Int*) = {var sum = 0;for(x<-a)sum += x;sum}


def out(a:Int){println("hello")}


lazy val a:Int = "hello world"

lazy val x = scala.io.Source.fromFile("d:\\tmp\\HelloScala.scala").mkString

try{
    "hello".toInt;
} catch {
    case _: Exception => print("xxx")
    case ex:java.io.IOException => print(ex)
}