A+ enrollment questionnaire - template for common questions
===========================================================

This repository contains a template for an A+ enrollment questionnaire.
Enrollment questionnaire is a normal A+ questionnaire that students must
complete when they enroll in the course in A+.

How to use this in your course
------------------------------

1. Ensure that the course is using an up-to-date version of the a-plus-rst-tools
   (this template defines the questionnaire in RST markup).
   The version matching the git checksum `7e18045` (15th Aug 2019) is at least new enough.
2. Modify the `conf.py` file in the root of your course repository.
   Add these definitions for the `course_key` and `rst_prolog` variables as well as
   `exclude_patterns`:

   ```python
   # The JavaScript code used by the enrollment questionnaire is hosted in the course repo,
   # so we need to know the course key in order to craft the URL of the JS.
   # This RST substitution is used to insert the script in the questionnaire RST code.
   course_key = 'default' # FIXME: use the correct key automatically
   rst_prolog = '''.. |enroll-js-script| raw:: html

     <script src="/static/{course_key}/_static/enrollmentquiz.js"></script>
   '''.format(course_key=course_key)
   
   # Add 'enrollment' to the exclude_patterns list. The list has probably
   # initially been defined with some elements.
   exclude_patterns = ['_build', '_data', 'enrollment']
   ```
   
   If the `exclude_patterns` variable is not defined correctly, compiling the
   course RST fails with an error (because the question templates should not
   be compiled independently; they are included into another RST file):
   
   ```
   File "/path/to/course/a-plus-rst-tools/directives/questionnaire.py", line 152, in create_question
       env.question_count += 1
   AttributeError: 'BuildEnvironment' object has no attribute 'question_count'
   ```

3. Add this aplus-enrollment-questionnaire repository as a git submodule to
   the root of the course repository. The name of the directory inside the
   course repository shall be `enrollment`. The submodule contains commonly
   used questions and files.

   ```
   git submodule add https://github.com/Aalto-Letech/aplus-enrollment-questionnaire.git enrollment
   ```

4. You need to add the enrollment questionnaire to the course contents as follows.
   
   1. Modify the `index.rst` file of the first exercise round (module).
   (Or a different module if you know what you are doing.)
   The file may have a different name since it is defined in the main `index.rst`
   of the course. The module index.rst defines the chapters of the module with
   the `toctree` directive. **Add** either the chapter `enrollment_en` or `enrollment_fi`
   (English or Finnish version) to the **end** of the first module in
   the module index.rst.
   2. **Copy** the corresponding file (`enrollment_en.rst` or `enrollment_fi.rst`)
   from the `enrollment` directory into the module directory.
   3. You may modify the questionnaire in that file if necessary.
      If the course is not offered to external students (who use Google login),
      then you may remove the questionnaire `enrollexternalexercise` from the RST file.

5. Copy the file `enrollment/_static/enrollmentquiz.js` into the path
   `_static/enrollmentquiz.js` so that the JS file is included in the static files
   of the course.

