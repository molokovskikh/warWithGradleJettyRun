**1)** Скачать gradle, по ссылке https://services.gradle.org/distributions/gradle-2.7-bin.zip (если gradle установлен и версия его не ниже **2.1**, то пропускаем этот пункт, версию можно проверить из консоли командой "*gradle --version*")

**2)** Распаковать локально **с:\gradle-2.7** (если gradle установлен и версия его не ниже **2.1**, то пропускаем этот пункт, версию можно проверить из консоли командой "*gradle --version*")

**3)** Добавить путь "**c:\gradle-2.7\bin**" в переменную среды **%PATH%** ($PATH). (Для Windows F5 на рабочем столе для обновления %PATH%) (если gradle установлен и версия его не ниже **2.1**, то пропускаем этот пункт, версию можно проверить из консоли командой "*gradle --version*")

**4)** Создать новый каталог **c:\JettyRunWar**

**5)** Создать файл **c:\JettyRunWar\build.gradle** со следующим содержимым:

```groovy
apply plugin: 'jetty'

def fileWar = file("$rootDir").listFiles().find { it.name.endsWith('.war') }

if(fileWar&&fileWar.exists()) {

task replaceWar(type:Copy){
	from fileWar
	into war.destinationDir
	rename (fileWar.name,war.archiveName)
}

war {
	dependsOn clean
	doLast{
		replaceWar.execute()
	}
}


jettyRunWar {
	contextPath 'webApp'
	/*
	//Если для запуска сервера требуются артефакты положите их в каталог libs
	additionalRuntimeJars = fileTree("$rootDir/libs").include('.jar')
	*/
	
			/*
	//Порт для запуска HttpListener'a можно уточнить
	httpPort=8081
	*/
}

defaultTasks 'jettyRunWar'
```
**6)** Выполнить в консоли **"cd c:\JettyRunWar && gradle wrapper"** (*в каталоге c:\JettyRunWar должны появится gradle,gradlew*)

**7)** Скопировать в **c:\JettyRunWar** файл **webApp.war**

**8)** Выполнить из консоли **gradlew**

**9)** Запустить броузер и проверить доступ по ссылке [http://localhost:8080/webApp](http://localhost:8080/webApp "Проверка запуска jettyRun")

**10)** Если есть зависимости времени выполнения, которые не включены в WAR-файл, вы можете создать каталог c:\JettyRunWar\libs , в который поместите эти Jar-файлы

Для теста можно скачать jenkins
http://mirror.yandex.ru/mirrors/jenkins/war/1.637/jenkins.war в каталог **c:\JettyRunWar**