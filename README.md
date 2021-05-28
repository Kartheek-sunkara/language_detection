# language_detection


https://stackoverflow.com/questions/39142778/python-how-to-determine-the-language
1. TextBlob.
Requires NLTK package, uses Google.

    from textblob import TextBlob
    b = TextBlob("bonjour")
    b.detect_language()
pip install textblob

Note: This solution requires internet access and Textblob is using Google Translate's language detector by calling the API.

2. Polyglot.
Requires numpy and some arcane libraries, unlikely to get it work for Windows. (For Windows, get an appropriate versions of PyICU, Morfessor and PyCLD2 from here, then just pip install downloaded_wheel.whl.) Able to detect texts with mixed languages.

    from polyglot.detect import Detector

    mixed_text = u"""
    China (simplified Chinese: 中国; traditional Chinese: 中國),
    officially the People's Republic of China (PRC), is a sovereign state
    located in East Asia.
    """
    for language in Detector(mixed_text).languages:
            print(language)

    # name: English     code: en       confidence:  87.0 read bytes:  1154
    # name: Chinese     code: zh_Hant  confidence:   5.0 read bytes:  1755
    # name: un          code: un       confidence:   0.0 read bytes:     0
pip install polyglot

To install the dependencies, run: sudo apt-get install python-numpy libicu-dev

Note: Polyglot is using pycld2, see https://github.com/aboSamoor/polyglot/blob/master/polyglot/detect/base.py#L72 for details.

3. chardet
Chardet has also a feature of detecting languages if there are character bytes in range (127-255]:

    >>> chardet.detect("Я люблю вкусные пампушки".encode('cp1251'))
    {'encoding': 'windows-1251', 'confidence': 0.9637267119204621, 'language': 'Russian'}
pip install chardet

4. langdetect
Requires large portions of text. It uses non-deterministic approach under the hood. That means you get different results for the same text sample. Docs say you have to use following code to make it determined:

    from langdetect import detect, DetectorFactory
    DetectorFactory.seed = 0
    detect('今一はお前さん')
pip install langdetect

5. guess_language
Can detect very short samples by using this spell checker with dictionaries.

pip install guess_language-spirit

6. langid
langid.py provides both module

    import langid
    langid.classify("This is a test")
    # ('en', -54.41310358047485)
and a command-line tool:

    $ langid < README.md
pip install langid

7. FastText
FastText is a text classifier, can be used to recognize 176 languages with a proper models for language classification. Download this model, then:

    import fasttext
    model = fasttext.load_model('lid.176.ftz')
    print(model.predict('الشمس تشرق', k=2))  # top 2 matching languages

    (('__label__ar', '__label__fa'), array([0.98124713, 0.01265871]))
pip install fasttext

8. pyCLD3
pycld3 is a neural network model for language identification. This package contains the inference code and a trained model.

    import cld3
    cld3.get_language("影響包含對氣候的變化以及自然資源的枯竭程度")

    LanguagePrediction(language='zh', probability=0.999969482421875, is_reliable=True, proportion=1.0)
pip install pycld3
