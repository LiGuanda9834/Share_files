# Share_files
const auto& fracints = lp->getFractionalIntegers();
  
  // do cloud check
  int up_skip_size = cloud_skip_up.size();
  int down_skip_size = cloud_skip_down.size();

  double minScore = mipsolver.mipdata_->feastol;

  std::unordered_set<int> cur_skip_up;
  std::unordered_set<int> cur_skip_down;

  std::for_each(cloud_skip_up.begin(), cloud_skip_up.end(), [&](int val){
      cur_skip_up.insert(val);
  });
  cout << endl;

  std::for_each(cloud_skip_down.begin(), cloud_skip_down.end(), [&](int val){
    cur_skip_down.insert(val);
  });
  cout << endl;

  cout << "skip up : ";
  for(auto& val : cloud_skip_up){
    std::cout << val << " ";
  }

  cout << endl << "skip down : ";
  for(auto& val : cloud_skip_down){
    std::cout << val << " ";
  }
  cout << endl;
  printf("\nTotal Skip %d up and %d down\n Now Check : ", up_skip_size, down_skip_size);
  int numfrac = fracints.size();
  for(int k = 0; k < numfrac; k++){
    HighsInt col = fracints[k].first;
    cout << "Col " << col << " : ";
    double fracval = fracints[k].second;
    bool do_skip = false;
    if(cur_skip_up.find(col) != cur_skip_up.end()){
      cout << "Skip up, \t";
      upscore[k] = minScore;
      upscorereliable[k] = true;
      do_skip = true;
    }
    if(cur_skip_down.find(col) != cur_skip_down.end()){
      cout << "Skip down, \t";
      downscore[k] = minScore;
      downscorereliable[k] = true;
      do_skip = true;
    }
    if(!do_skip){
      cout << "Do not skip";
    }
    cout << endl;
  }
