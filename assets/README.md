# Assets

This directory holds local game assets only. Nothing in it is committed to Git.

- Assets are reused from the legacy game and must never be redistributed through the repository.
- Assets are shared between contributors as i2pd torrents (i2psnark-style), never over clearnet.
- The repository tracks only the signed asset manifest: asset IDs, hashes, and versions.
- Code must not reference asset file names, paths, or literal asset identifiers. Code references assets only through registry IDs resolved at runtime from the local manifest.
- Tests use synthetic fixtures and never require real game assets.

Every contributor works with a local checkout of the assets next to this file. The directory layout is created locally when the asset torrent is fetched.
