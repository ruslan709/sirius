Здравствуйте дорогие коллеги!!!


Этот код использует модель градиентного бустинга для рекомендации фильмов
на основе их сходства с другими фильмами. Он также строит график, который показывает релевантность рекомендуемых фильмов.
 В конце кода выводятся сами рекомендации, их релевантность и объяснения, почему эти фильмы были рекомендованы.



# Создаем модель градиентного бустинга
model = GradientBoostingRegressor()

# Определяем целевую переменную и обучаем модель
target = ratings['rating']
model.fit(ratings, target)


 # Получаем сходство текущего фильма с остальными фильмами
    similarities = cosine_similarity(movies.loc[movie_id].values.reshape(1, -1), movies.values)
    similar_movies_indexes = similarities.argsort()[0][-n-1:-1]
    


 # Получаем рекомендуемые фильмы на основе сходства
    recommended_movies = movies.iloc[similar_movies_indexes]



# Получаем значения релевантности для рекомендуемых фильмов
    relevance_values = similarities[0][similar_movies_indexes]
   


 # Строим график релевантности фильмов
    plt.bar(recommended_movies['movie_id'], relevance_values, color='skyblue')
    plt.xlabel('Фильмы')
    plt.ylabel('Значение релевантности')
    plt.title('Релевантность рекомендуемых фильмов')
    plt.show()

 # Создаем список объяснений для рекомендаций
    explanations = []
    for index, movie in recommended_movies.iterrows():
        explanations.append(f"потому что вы просмотрели и оценили фильм {movie_id} на 5 баллов")

    return recommended_movies, relevance_values, explanations

# Получаем рекомендации для фильма с id 123
recommended, relevance, explanations = recommend_movies(movie_id=123, n=5)

print("Рекомендуемые фильмы:")
print(recommended)
print("Значения релевантности:")
print(relevance)
print("Объяснения:")
for explanation in explanations:
    print(explanation)