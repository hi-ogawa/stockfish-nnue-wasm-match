# stockfish-nnue.wasm match

Engine match testing for [stockfish-nnue.wasm](https://github.com/hi-ogawa/Stockfish)
using [Github Actions workflow_dispatch](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#workflow_dispatch).

```
# Install two versions of engines
npm i engine1@npm:stockfish-nnue.wasm@0.0.2-dev.7c1b679a
npm i engine2@npm:stockfish-nnue.wasm@1.0.0-dev.e3613959

# (or install local versions)
npm i engine1@../stockfish-nnue-wasm-v1/src/emscripten/public
npm i engine2@../stockfish-nnue-wasm-v2/src/emscripten/public

# Download opening book
wget https://github.com/official-stockfish/books/raw/master/noob_3moves.epd.zip
unzip noob_3moves.epd.zip

# Run cutechess
ROUNDS=10
TC="60+0.6"
cutechess-cli -tournament round-robin -rounds $ROUNDS \
  -openings file=noob_3moves.epd format=epd policy=round -repeat -games 2 \
  -draw movenumber=40 movecount=4 score=10 \
  -resign movecount=4 score=1000 \
  -pgnout result-$(date +%F-%H-%M-%S).pgn \
  -each proto=uci \
  -engine name=engine1 tc=$TC cmd=node arg=$PWD/node_modules/engine1/uci.js \
  -engine name=engine2 tc=$TC cmd=node arg=$PWD/node_modules/engine2/uci.js
```
