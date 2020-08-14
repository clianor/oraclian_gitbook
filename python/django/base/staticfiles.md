# Static 파일 설정

기존에 장고 static file 설정을 하던때 아래와 같이 셋팅했었다.

```python
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static')
]
```

하지만 이제는 아래와 같이 셋팅하도록 바뀐거 같다.

```python
STATICFILES_DIRS = [
    BASE_DIR / "static",
]
```



