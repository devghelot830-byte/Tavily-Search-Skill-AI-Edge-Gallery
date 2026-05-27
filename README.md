# Tavily-Search-Skill-AI-Edge-Gallery
This is a Tavily Search skill Ive named live-info for Google's AI Edge Gallery. It requires a Tavily API key to work (you can register for a free key).

when you askThis GitHub commit changes how the LiteRT-LM (Google AI Edge) Python package is built and bundled. It moves a key file generation target directly into the builder directory to simplify PyPI packaging.
## Main Objective
The main goal of this commit is to simplify dependencies for PyPI packaging by moving the FlatBuffer Python library generation target (:litertlm_header_schema_py) inside python/litert_lm_builder/BUILD.
------------------------------
## Key Changes Broken Down## 1. Build Architecture (BUILD Files)

* New Target Added: The flatbuffer_py_library target is now declared directly inside python/litert_lm_builder/BUILD, fetching the flatbuffer schema (.fbs) file from its original location via //schema/core:litertlm_header_schema.fbs.
* Local Dependency Swap: All internal tools (litertlm_core, litertlm_core_test, litertlm_peek, and the main builder library) now rely on the local :litertlm_header_schema_py build target instead of the external //schema/core:litertlm_header_schema_py path.
* Schema Visibility: Added exports_files(["litertlm_header_schema.fbs"]) inside schema/core/BUILD so the .fbs file can be accessed across packages.

## 2. Import Path Cleanup (.py Files)
Because the generated FlatBuffer code is now built locally within the builder, all import lines in the source files were updated to match the new flat directory layout:

* Old: from schema.core import litertlm_header_schema_py_generated as schema
* New: from litert_lm_builder import litertlm_header_schema_py_generated as schema
* This affects litertlm_builder.py, litertlm_core.py, litertlm_core_test.py, and litertlm_peek.py.

## 3. Streamlining the Deployment Script (bundle_pypi_package.sh)

* Simpler Staging: The build script no longer needs to create nested directories like schema/core or touch temporary __init__.py files to fake python packages during the build.
* Direct Copying: The script now targets the locally generated *_generated.py build binaries from the local folder, ensuring a cleaner, flatter structure inside the final PyPI wheel package.

------------------------------
Would you like help debugging a build failure related to this Bazel change, or do you need to update a local branch to match these new import paths?

 a question in which it needs extra information the skill will be involed and used.

i tested this with Google's Gemma-4-alE2B model.




