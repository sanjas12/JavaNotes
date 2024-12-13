интерфейс `Function` из пакета `java.util.function` представляет собой функциональный интерфейс, который принимает один аргумент и возвращает результат. Это делает его идеальным для преобразования данных.

		Function<String, String> function = new Function<String, String>() {
            public String  apply(String s){
                return s.toUpperCase();
            }
        };

       return strings.map(function);
       